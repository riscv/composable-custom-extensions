.. SPDX-License-Identifier: GPL-2.0

=====================================================
Composable Custom Extensions Support for RISC-V Linux
=====================================================

This document briefly outlines the interface provided to userspace by
Linux in order to support the use of the RISC-V Composable Custom
Extensions.

1. prctl() Interface
--------------------

One prctl() call added to allow queries related to the UUID that
identifies a Composable Custom Extension.  Two additional prctl()
calls are added to control enablement of a Composable Custom Extension
for userspace.

New processes start with all custom extensions disabled.

* prctl(PR_RISCV_CX_QUERY, unsigned long *cxid, uuid_t *uuid, unsigned int flags)

    Query CX UUID, returned in `uuid`, from the given `cxid`.

    If flags is set to `RISCV_CX_QUERY_CXID`, will return `cxid` for
    the corresponding `uuid`.

* prctl(PR_RISCV_CX_ENABLE, unsigned long cxid, unsigned long *sel, unsigned int flags)

    Request enabling the composable custom extension, specified by
    `cxid`, for the current process.  If successful, `sel` is returned
    with a CX selector that may be used by the `cxsel` instruction to
    select the specified custom extension.

    A given custom extension may be enabled multiple times.  Each
    enable must be matched with a corresponding disable for the
    extension to be disabled.

    `flags` is currently unused and should be set to zero.

* prctl(PR_RISCV_CX_DISABLE, unsigned long sel)

    Request disabling a custom extension, specified by `sel`, the
    selector previously returned by a corresponding enable call.

2. System Call Behavior
-----------------------

The Composable Custom Extension framework state is preserved across
system calls.  Custom Extension state is also preserved across system
calls.
