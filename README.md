## Building a libc offset and ld database

Fetch all the configured libc and ld versions and extract the symbol offsets.

It will not download anything twice, so you can also use it to update your
database:

    $ ./get

You can also add a custom libc and ld to your database.

    $ ./add /usr/lib/libc-2.21.so /usr/lib/linux-ld.so.2

Find all the libc's in the database that have the given names at the given
addresses. Only the last 12 bits are checked, because randomization usually
works on page size level.

    $ ./find printf 260 puts f30
    archive-glibc (id libc6_2.19-10ubuntu2_i386)

Find a libc from the leaked return address into `__libc_start_main`.

    $ ./find __libc_start_main_ret a83
    ubuntu-trusty-i386-libc6 (id libc6_2.19-0ubuntu6.6_i386)
    archive-eglibc (id libc6_2.19-0ubuntu6_i386)
    ubuntu-utopic-i386-libc6 (id libc6_2.19-10ubuntu2.3_i386)
    archive-glibc (id libc6_2.19-10ubuntu2_i386)
    archive-glibc (id libc6_2.19-15ubuntu2_i386)

Dump some useful offsets, given a libc ID. You can also provide your own names
to dump.

    $ ./dump libc6_2.19-0ubuntu6.6_i386
    offset___libc_start_main_ret = 0x19a83
    offset_system = 0x00040190
    offset_dup2 = 0x000db590
    offset_recv = 0x000ed2d0
    offset_str_bin_sh = 0x160a24

Check whether a library is already in the database.

    $ ./identify /usr/lib/libc.so.6
    id local-f706181f06104ef6c7008c066290ea47aa4a82c5

Grab the libc and ld-linux files and place them in a specific directory.

    $ ./grab libc6_2.19-10ubuntu2.3_i386 /tmp/

Delete everything from the database.

    $ ./clean
