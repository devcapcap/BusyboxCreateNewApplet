# Create new menu and add new applet

0. clone busybox source code from the official git
	* git clone https://git.busybox.net/busybox/
	* git checkout remotes/origin/1_36_stable  (for example the stable version  1.36)
	* cd busybox

1. create a directory in the root busybox directory with the commandline -> mkdir custom
2. create a new file with the named hello.c in the directory created above
3. Add the code below to your hello.c file

	```
	/* vi: set sw=4 ts=4: */
	/*
	 * Copyright (C) 2024 your name/first name <yourAddress@domain.com>
	 *
	 * Licensed under GPLv2, see file LICENSE in this source tree.
	 */
	//config:config HELLO
	//config:	bool "hello"
	//config:	default y
	//config:	help
	//config:	Print hello word.
	//config:

	//applet:IF_HELLO(APPLET(hello, BB_DIR_USR_BIN, BB_SUID_DROP))

	//kbuild:lib-$(CONFIG_HELLO) += hello.o

	//usage:#define hello_trivial_usage NOUSAGE_STR
	//usage:#define hello_full_usage ""

	#include "libbb.h"

	int hello_main(int argc, char **argv) MAIN_EXTERNALLY_VISIBLE;
	int hello_main(int argc UNUSED_PARAM, char **argv UNUSED_PARAM)
	{
		printf("hello world\n");
		return EXIT_SUCCESS;
	}

	```

4. Add two files named (with this 2 files you have after do a make defconfig 2 new files called Config.in and Kbuild)
	* Kbuild.src and the content below
		```
		# Makefile for busybox
		#
		# Copyright (C) 2024 your name/first name <yourAddress@domain.com>
		#
		# Licensed under GPLv2, see file LICENSE in this source tree.

		lib-y:=

		INSERT

		```
	* Config.src and the content below

		```
		#
		# For a description of the syntax of this configuration file,
		# see docs/Kconfig-language.txt.
		#

		menu "Custom"

		INSERT

		endmenu

		```

6. Modifify the Config.in located in the root directory of busybox source code directory and add at the end of the file 
	* "source custom/Config.in" (respect the alphabetic order without quotes)

7. Modify the Makefile located in the root directory of busybox source code directory and add the line below "-libs-y : ="
	* "custom/ \" (without quotes)

8. compile
 	* make defconfig or another
	* make
	* chmod +x busybox and ./busybox hello

