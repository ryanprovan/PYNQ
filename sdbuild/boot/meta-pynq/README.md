meta-pynq
=========

Yocto (Scarthgap) layer used by the AMD EDF builds in `sdbuild`. It is added
to the EDF build by `sdbuild/scripts/build_edf_boot.sh` and
`sdbuild/scripts/build_edf_remote_rootfs.sh`.

Boot artefacts
--------------

For every board, the layer adapts AMD's kernel, device tree and u-boot:

 * `recipes-bsp/device-tree`: adds the PYNQ UIO and zocl nodes and the kernel
   bootargs, appends the board's `edf_bsp/board.dtsi`, and records the board
   name in `/chosen/pynq_board`
 * `recipes-kernel/linux`: PYNQ kernel config fragments and patches, plus the
   board's `edf_bsp/kernel.cfg`
 * `recipes-bsp/u-boot`: patches and config fragments from the board's
   `edf_bsp/u-boot/`
 * `recipes-xrt/zocl`: the zocl kernel module, pinned to the XRT release used
   in the root filesystem

PYNQ.remote
-----------

`pynq-remote-image` is the PYNQ.remote root filesystem. It installs:

 * `pynq-cpp`: the `pynq-remote` gRPC server. RFSoC boards enable the `rfsoc`
   PACKAGECONFIG to add the xrfdc and xrfclk services
 * `pynq-remote-network`: DHCP on wired interfaces through systemd-networkd
 * `pynq-remote-selftest`: the on-target self-test

`recipes-bsp/librfdc` installs `librfdc.so` in the runtime package, because
`pynq-remote` loads it by that name.
