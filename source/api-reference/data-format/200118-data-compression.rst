﻿Data compression
==================
Over lossless connection we have to pass only initial state and later on just changes. 
To compress the only changes we keep record of previous data sent in transfer buffer. 

delta[i] = data[i] – prev_data[i]

This is done just with 8 bit integers, overflow ignored. Typically most of diff[i] values 
are zeroes, since in most cases much of data does not change. So we use zero-run compression:

1. Repeat while we have data left. Interrupt with -1 if compressed data is as long or longer than original data.
2. Count how many nonzero bytes there are starting from current byte p. Count up to maximum 255 bytes. Do not interrupt count to one stand alone zero
3. Write number of nonzero bytes.
4. Write nonzero bytes. 
5. Calculate number of zero by
6. Count how many zero bytes there are starting from current byte p. Count up to maximum 255 bytes.
7. Write number of zero bytes.
8. Go back to step two.

.. code-block:: c

    /**
    ****************************************************************************************************

    @brief Compress data.
    @anchor ioc_compress

    The ioc_compress() function compresses data from source buffer into destination buffer.
    Source data is from start_address

    @param  srcbuf Source buffer pointer.
    @param  start_addr At entry, index of the first byte in source buffer to compress. At exit index
            of first byte which was left uncompressed. If whole source buffer was compressed,
            this is end_addr + 1.
    @param  end_addr Index of last byte in source buffer to compress.
    @param  dst Pointer to destination buffer.
    @param  dst_sz Maximum number of bytes to store in destination buffer. The compression
            is interrupted when up to this many bytes have been stored in destination buffer.
    @return Number of bytes in destination buffer or -1 if is not compressed (longer than original)

    ****************************************************************************************************
    */
    os_int ioc_compress(
        os_char *srcbuf,
        os_int *start_addr,
        os_int end_addr,
        os_char *dst,
        os_memsz dst_sz)
    {
        os_char
            *dst_start,
            *dst_end,
            *p,
            *start;

        os_int
            bytes,
            count,
            dst_count,
            max_count,
            rval;

        /* If less than three bytes, do not even try to compress.
        */
        if (end_addr - *start_addr < 3) return -1;

        dst_start = dst;
        dst_end = dst + dst_sz;

        p = &srcbuf[*start_addr];
        bytes = end_addr - *start_addr + 1;

        while (bytes > 0)
        {
            start = p;
            count = 0;
            max_count = bytes;
            if (max_count > 255) max_count = 255;
            while ((p[0] || p[count < max_count-1 ? 1 : 0]) && count < max_count)
            {
                count++;
                p++;
            }
            bytes -= count;

            if (dst + count >= dst_end)
            {
                p = start;
                break;
            }

            *(dst++) = count;
            while (count--) *(dst++) = *(start++);
            if (bytes == 0) break;

            start = p;
            count = 0;
            max_count = bytes;
            if (max_count > 255) max_count = 255;
            while (*p== 0 && count < max_count)
            {
                count++;
                p++;
            }
            bytes -= count;

            if (dst >= dst_end)
            {
                p = start;
                break;
            }
            *(dst++) = count;
        }

        count = (os_int)(p - &srcbuf[*start_addr]);
        dst_count = (os_int)(dst - dst_start);

        rval = dst_count < count ? dst_count : -1;
        if (rval >= 0) *start_addr = (os_int)(p - srcbuf);
        return rval;
    }


The outgoing message will contain “delta encoding” and “zero run compression” bits in header. 
If compression result would be as long or longer than data in uncompressed format, compression is not done.
The delta encoding is not done at least for the first snapshot to send. Data received should use these bits
 to detect if data is delta encoded or compressed to read it.

Data decompression is simpler to implement than compression:

1. Start from first source byte to process.
2. Quit if we have processed all source data.
3. Get next source byte in n. It is count of real data bytes.
4. Move n data bytes from source to destination.
5. Quit if we have processed all source data.
6. Get next source byte in n. It is count of zero bytes.
7. Write n zero bytes to destination 
8. Back to step 2.
       
In addition we must check that we do not overflow destination buffer, in case our source data is 
erroneous. Undoing delta encoding is also simple. We add received delta values to previous values.

data[i] = data[i] + delta[i]

This is again done just with 8 bit integers, overflow ignored. 

.. code-block:: c

    /**
    ****************************************************************************************************

    @brief Uncompress data.
    @anchor ioc_compress

    The ioc_uncompress() function uncompresses data from source buffer into destination buffer.
    Delta encoding is taken care of if set in flags.

    1. Start from first source byte to process.
    2. Quit if we have processed all source data.
    3. Get next source byte in n. It is count of real data bytes.
    4. Move n data bytes from source to destination.
    5. Quit if we have processed all source data.
    6. Get next source byte in n. It is count of zero bytes.
    7. Write n zero bytes to destination
    8. Back to step 2.

    @param   src Source buffer pointer.
    @param   bytes Number of source bytes.
    @param   end_addr Index of last byte in source buffer to compress.
    @param   dst Pointer to destination buffer.
    @param   dst_sz Maximum number of bytes to store in destination buffer.
    @return  Number of destination bytes if uncompression was successful. -1 indicates failed
             decompression (source data is corrupted)

    ****************************************************************************************************
    */
    os_int ioc_uncompress(
        os_char *src,
        os_int src_bytes,
        os_char *dst,
        os_memsz dst_sz,
        os_uchar flags)
    {
        os_char
            *src_end,
            *dst_start,
            *dst_end;

        os_uint
            n;

        src_end = src + src_bytes;
        dst_start = dst;
        dst_end = dst + dst_sz;

        if ((flags & IOC_COMPRESESSED) == 0)
        {
            n = src_bytes;
            if (src + n > src_end) return -1;
            if (dst + n > dst_end) n = (os_uint)(dst_end - dst);

            if (flags & IOC_DELTA_ENCODED)
            {
                while (n--)
                {
                    *(dst++) += *(src++);
                }
            }
            else
            {
                while (n--)
                {
                    *(dst++) = *(src++);
                }
            }

            return (os_int)(dst - dst_start);
        }


        while (src < src_end && dst < dst_end)
        {
            n = (os_uchar)*(src++);
            if (src + n > src_end) return -1;
            if (dst + n > dst_end) n = (os_uint)(dst_end - dst);

            if (flags & IOC_DELTA_ENCODED)
            {
                while (n--)
                {
                    *(dst++) += *(src++);
                }
            }
            else
            {
                while (n--)
                {
                    *(dst++) = *(src++);
                }
            }

            if (src >= src_end || dst >= dst_end) break;
            n = (os_uchar)*(src++);
            if (dst + n > dst_end) n = (os_uint)(dst_end - dst);

            if (flags & IOC_DELTA_ENCODED)
            {
                dst += n;
            }
            else
            {
                while (n--)
                {
                    *(dst++) = 0;
                }
            }
        }

        return (os_int)(dst - dst_start);
    }


200118, updated 18.5.2019/pekka

