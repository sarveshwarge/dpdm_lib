#   BSD LICENSE
#
#   Copyright(c) 2017- Intel Corporation. All rights reserved.
#   All rights reserved.
#
#   Redistribution and use in source and binary forms, with or without
#   modification, are permitted provided that the following conditions
#   are met:
#
#     * Redistributions of source code must retain the above copyright
#       notice, this list of conditions and the following disclaimer.
#     * Redistributions in binary form must reproduce the above copyright
#       notice, this list of conditions and the following disclaimer in
#       the documentation and/or other materials provided with the
#       distribution.
#     * Neither the name of Intel Corporation nor the names of its
#       contributors may be used to endorse or promote products derived
#       from this software without specific prior written permission.
#
#   THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
#   "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
#   LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
#   A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
#   OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
#   SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
#   LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
#   DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
#   THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
#   (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
#   OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

# define the C compiler to use
CC = gcc

# define any compile-time flags
CFLAGS = -O2 -D_FORTIFY_SOURCE=2 -msse4.2 -Wall -DX722_A0_SUPPORT -DPF_DRIVER -DVF_DRIVER -DINTEGRATED_VF

# start from DPDK-17.11, the rte_config.h is
# added from Makefile
CFLAGS += -include $(RTE_SDK)/$(RTE_TARGET)/include/rte_config.h

# add defense attacker check for compile/link
CFLAGS += ${stack_protector}
CFLAGS += -fPIE -fPIC
CFLAGS += -Wformat -Wformat-security

# define any directories containing header files other than /usr/include
#
INCLUDES = -I$(RTE_SDK)/$(RTE_TARGET)/include -I$(RTE_SDKEX)/build/include
INCLUDES += -I$(RTE_SDK)/drivers/net/i40e -I$(RTE_SDKEX)/drivers/net/i40e_ex
INCLUDES += -I$(VNI_NETDEV)

# define library paths in addition to /usr/lib
LFLAGS = -L$(RTE_SDK)/$(RTE_TARGET)/lib -L$(RTE_SDKEX)/lib/librte_ether_ex

# define any libraries to link into executable:
LIBS = -ldpdk -lrte_ether_ex

# define the C source files
SRCS = i40e_ethdev_ex.c i40evf_ethdev_ex.c

# define the C object files 
OBJS = $(SRCS:.c=.o)

# define the library file 
MAIN = librte_pmd_i40e_ex.a

.PHONY: depend clean

all: $(MAIN)
	@echo

.c.o: $(SRCS)
	$(CC) $(CFLAGS) $(INCLUDES) $(LFLAGS) $(LIBS) -c $<  -o $@

librte_pmd_i40e_ex.a: $(OBJS)
	ar rcs $@ $(OBJS)

clean:
	$(RM) *.o *~ $(MAIN)

depend: $(SRCS)
	makedepend $(INCLUDES) $^

# DO NOT DELETE THIS LINE -- make depend needs it
