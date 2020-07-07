    *** Compiler: arm-xilinx-linux-gnueabi-gcc (GCC) 9.2.0 ***                                                              < <  
  < <                                                          General setup  --->                                                                                                     < <  
  < <                                                      -*- Patch physical to virtual translations at runtime                                                                       < <  
  < <                                                          System Type  --->                                                                                                       < <  
  < <                                                          Bus support  --->                                                                                                       < <  
  < <                                                          Kernel Features  --->                                                                                                   < <  
  < <                                                          Boot options  --->                                                                                                      < <  
  < <                                                          CPU Power Management  --->                                                                                              < <  
  < <                                                          Floating point emulation  --->                                                                                          < <  
  < <                                                          Power management options  --->                                                                                          < <  
  < <                                                          Firmware Drivers  --->                                                                                                  < <  
  < <                                                      [ ] ARM Accelerated Cryptographic Algorithms  ----                                                                          < <  
  < <                                                      [ ] Virtualization  ----                                                                                                    < <  
  < <                                                          General architecture-dependent options  --->                                                                            < <  
  < <                                                      [*] Enable loadable module support  --->                                                                                    < <  
  < <                                                      [*] Enable the block layer  --->                                                                                            < <  
  < <                                                          IO Schedulers  --->                                                                                                     < <  
  < <                                                          Executable file formats  --->                                                                                           < <  
  < <                                                          Memory Management options  --->                                                                                         < <  
  < <                                                      [*] Networking support  --->                                                                                                < <  
  < <                                                          Device Drivers  --->                                                                                                    < <  
  < <                                                          File systems  --->                                                                                                      < <  
  < <                                                          Security options  --->                                                                                                  < <  
  < <                                                      -*- Cryptographic API  --->                                                                                                 < <  
  < <                                                          Library routines  --->                                                                                                  < <  
  < <                                                          Kernel hacking  --->
  
  
  
  
General Setup-------------------------------------------------------------------------------------------------------------------------------------------------------------------


 [ ] Compile also drivers which will not load                                                                                < <  
  < <                                                      [ ] Compile test headers that should be standalone compilable                                                               < <  
  < <                                                      (-xilinx-v2020.1) Local version - append to kernel release                                                                  < <  
  < <                                                      [*] Automatically append version information to the version string                                                          < <  
  < <                                                      ()  Build ID Salt                                                                                                           < <  
  < <                                                          Kernel compression mode (Gzip)  --->                                                                                    < <  
  < <                                                      ((none)) Default hostname                                                                                                   < <  
  < <                                                      [*] Support for paging of anonymous memory (swap)                                                                           < <  
  < <                                                      [*] System V IPC                                                                                                            < <  
  < <                                                      [ ] POSIX Message Queues                                                                                                    < <  
  < <                                                      [*] Enable process_vm_readv/writev syscalls                                                                                 < <  
  < <                                                      [ ] uselib syscall                                                                                                          < <  
  < <                                                      [ ] Auditing support                                                                                                        < <  
  < <                                                          IRQ subsystem  --->                                                                                                     < <  
  < <                                                          Timers subsystem  --->                                                                                                  < <  
  < <                                                          Preemption Model (Preemptible Kernel (Low-Latency Desktop))  --->                                                       < <  
  < <                                                          CPU/Task time and stats accounting  --->                                                                                < <  
  < <                                                      [*] CPU isolation                                                                                                           < <  
  < <                                                          RCU Subsystem  --->                                                                                                     < <  
  < <                                                      <*> Kernel .config support                                                                                                  < <  
  < <                                                      [*]   Enable access to .config through /proc/config.gz                                                                      < <  
  < <                                                      < > Enable kernel headers through /sys/kernel/kheaders.tar.xz                                                               < <  
  < <                                                      (14) Kernel log buffer size (16 => 64KB, 17 => 128KB)                                                                       < <  
  < <                                                      (12) CPU kernel log buffer size contribution (13 => 8 KB, 17 => 128KB)                                                      < <  
  < <                                                      (13) Temporary per-CPU printk log buffer size (12 => 4KB, 13 => 8KB)                                                        < <  
  < <                                                          Scheduler features  ----                                                                                                < <  
  < <                                                      [*] Control Group support  --->                                                                                             < <  
  < <                                                      [*] Namespaces support  --->                                                                                                < <  
  < <                                                      [*] Checkpoint/restore support                                                                                              < <  
  < <                                                      [ ] Automatic process group scheduling                                                                                      < <  
  < <                                                      [ ] Enable deprecated sysfs features to support old userspace tools                                                         < <  
  < <                                                      [ ] Kernel->user space relay support (formerly relayfs)                                                                     < <  
  < <                                                      [*] Initial RAM filesystem and RAM disk (initramfs/initrd) support                                                          < <  
  < <                                                      ()    Initramfs source file(s)                                                                                              < <  
  < <                                                      [*]   Support initial ramdisk/ramfs compressed using gzip                                                                   < <  
   < <                                                      ()    Initramfs source file(s)                                                                                              < <  
  < <                                                      [*]   Support initial ramdisk/ramfs compressed using gzip                                                                   < <  
  < <                                                      [*]   Support initial ramdisk/ramfs compressed using bzip2                                                                  < <  
  < <                                                      [*]   Support initial ramdisk/ramfs compressed using LZMA                                                                   < <  
  < <                                                      [*]   Support initial ramdisk/ramfs compressed using XZ                                                                     < <  
  < <                                                      [*]   Support initial ramdisk/ramfs compressed using LZO                                                                    < <  
  < <                                                      [*]   Support initial ramdisk/ramfs compressed using LZ4                                                                    < <  
  < <                                                          Compiler optimization level (Optimize for size (-Os))  --->                                                             < <  
  < <                                                      -*- Configure standard kernel features (expert users)  --->                                                                 < <  
  < <                                                      [ ] Enable bpf() system call                                                                                                < <  
  < <                                                      [ ] Enable userfaultfd() system call                                                                                        < <  
  < <                                                      [*] Enable rseq() system call                                                                                               < <  
  < <                                                      [ ]   Enabled debugging of rseq() system call                                                                               < <  
  < <                                                      [*] Embedded system                                                                                                         < <  
  < <                                                      [ ] PC/104 support                                                                                                          < <  
  < <                                                          Kernel Performance Events And Counters  --->                                                                            < <  
  < <                                                      [*] Enable VM event counters for /proc/vmstat                                                                               < <  
  < <                                                      [*] Disable heap randomization                                                                                              < <  
  < <                                                          Choose SLAB allocator (SLAB)  --->                                                                                      < <  
  < <                                                      [*] Allow slab caches to be merged                                                                                          < <  
  < <                                                      [ ] SLAB freelist randomization                                                                                             < <  
  < <                                                      [ ] Page allocator randomization                                                                                            < <  
  < <                                                      [ ] Profiling support
  
  
  
 System Type-------------------------------------------------------------------------------------------------------------------------------------------------------------------
  
  
   @^<                                                      [*] MMU-based Paged Memory Management Support                                                                               < <  
  < <                                                          ARM system type (Allow multiple platforms to be selected)  --->                                                         < <  
  < <                                                          Multiple platform selection  --->                                                                                       < <  
  < <                                                      [ ] Dummy Virtual Machine                                                                                                   < <  
  < <                                                      [ ] Actions Semi SoCs  ----                                                                                                 < <  
  < <                                                      [ ] Annapurna Labs Alpine platform                                                                                          < <  
  < <                                                      [ ] Axis Communications ARM based ARTPEC SoCs  ----                                                                         < <  
  < <                                                      [ ] Aspeed BMC architectures  ----                                                                                          < <  
  < <                                                      [ ] AT91/Microchip SoCs  ----                                                                                               < <  
  < <                                                      [ ] Broadcom SoC Support  ----                                                                                              < <  
  < <                                                      [ ] Marvell Berlin SoCs  ----                                                                                               < <  
  < <                                                      [ ] Conexant Digicolor SoC Support                                                                                          < <  
  < <                                                      [ ] Samsung EXYNOS  ----                                                                                                    < <  
  < <                                                      [ ] Calxeda ECX-1000/2000 (Highbank/Midway)                                                                                 < <  
  < <                                                      [ ] Hisilicon SoC Support                                                                                                   < <  
  < <                                                      [ ] Freescale i.MX family  ----                                                                                             < <  
  < <                                                      [ ] Texas Instruments Keystone Devices                                                                                      < <  
  < <                                                      [ ] MediaTek SoC Support  ----                                                                                              < <  
  < <                                                      [ ] Amlogic Meson SoCs  ----                                                                                                < <  
  < <                                                      [ ] Socionext Milbeaut SoCs  ----                                                                                           < <  
  < <                                                      [ ] Marvell PXA168/910/MMP2  ----                                                                                           < <  
  < <                                                      [ ] Marvell Engineering Business Unit (MVEBU) SoCs  ----                                                                    < <  
  < <                                                      [ ] Nuvoton NPCM Architecture  ----                                                                                         < <  
  < <                                                          TI OMAP/AM/DM/DRA Family  --->                                                                                          < <  
  < <                                                      [ ] CSR SiRF  ----                                                                                                          < <  
  < <                                                      [ ] Qualcomm Support  ----                                                                                                  < <  
  < <                                                      [ ] RDA Micro SoCs  ----                                                                                                    < <  
  < <                                                      [ ] ARM Ltd. RealView family  ----                                                                                          < <  
  < <                                                      [ ] Rockchip RK2928 and RK3xxx SOCs                                                                                         < <  
  < <                                                      [ ] Samsung S5PV210/S5PC110                                                                                                 < <  
  < <                                                      [ ] Renesas ARM SoCs  ----                                                                                                  < <  
  < <                                                      [ ] Altera SOCFPGA family  ----                                                                                             < <  
  < <                                                      [ ] ST SPEAr Family  ----                                                                                                   < <  
  < <                                                      [ ] STMicroelectronics Consumer Electronics SOCs  ----                                                                      < <  
  < <                                                      [ ] STMicroelectronics STM32 family  ----                                                                                   < <  
  
    < <                                                      [ ] Allwinner SoCs  ----                                                                                                    < <  
  < <                                                      [ ] Sigma Designs Tango4 (SMP87xx)                                                                                          < <  
  < <                                                      [ ] NVIDIA Tegra  ----                                                                                                      < <  
  < <                                                      [ ] Socionext UniPhier SoCs                                                                                                 < <  
  < <                                                      [ ] ST-Ericsson U8500 Series  ----                                                                                          < <  
  < <                                                      [*] ARM Ltd. Versatile Express family  --->                                                                                 < <  
  < <                                                      [ ] WonderMedia WM8850                                                                                                      < <  
  < <                                                      [ ] ZTE ZX family  ----                                                                                                     < <  
  < <                                                      [*] Xilinx Zynq ARM Cortex A9 Platform                                                                                      < <  
  < <                                                            Xilinx Specific Options  --->                                                                                         < <  
  < <                                                          *** Processor Type ***                                                                                                  < <  
  < <                                                          *** Processor Features ***                                                                                              < <  
  < <                                                      [ ] Support for the Large Physical Address Extension                                                                        < <  
  < <                                                      [*] Support Thumb user binaries                                                                                             < <  
  < <                                                      [ ] Enable ThumbEE CPU extension                                                                                            < <  
  < <                                                      [ ] Build big-endian kernel                                                                                                 < <  
  < <                                                      [ ] Disable I-Cache (I-bit)                                                                                                 < <  
  < <                                                      [ ] Workaround for I-Cache line size mismatch between CPU cores                                                             < <  
  < <                                                      [ ] Disable branch prediction                                                                                               < <  
  < <                                                      [*] Harden the branch predictor against aliasing attacks                                                                    < <  
  < <                                                      [*] Enable kuser helpers in vector page                                                                                     < <  
  < <                                                      [ ] Enable VDSO for acceleration of some system calls                                                                       < <  
  < <                                                      [*] Enable the L2x0 outer cache controller                                                                                  < <  
  < <                                                      [ ]   L2x0 performance monitor support                                                                                      < <  
  < <                                                      [*]   PL310 errata: Clean & Invalidate maintenance operations do not invalidate clean lines                                 < <  
  < <                                                      [*]   PL310 errata: Background Clean & Invalidate by Way operation can cause data corruption                                < <  
  < <                                                      -*-   PL310 errata: cache sync operation may be faulty                                                                      < <  
  < <                                                      [*]   PL310 errata: no automatic Store Buffer drain                                                                         < <  
  < <                                                      [*] Make rodata strictly non-executable                                                                                     < <  
  < <                                                      [ ] ARM errata: Stale prediction on replaced interworking branch                                                            < <  
  < <                                                      -*- ARM errata: LoUIS bit field in CLIDR register is incorrect                                                              < <  
  < <                                                      -*- ARM errata: TLBIASIDIS and TLBIMVAIS operations can broadcast a faulty ASID                                             < <  
  < <                                                      [*] ARM errata: possible faulty MMU translations following an ASID switch                                                   < <  
  < <                                                      [*] ARM errata: no automatic Store Buffer drain                                                                             < <  
    < <                                                      [*] ARM errata: no automatic Store Buffer drain                                                                             < <  
  < <                                                      [*] ARM errata: Data cache line maintenance operation by MVA may not succeed                                                < <  
  < <                                                      [*] ARM errata: A data cache maintenance operation which aborts, might lead to deadlock                                     < <  
  < <                                                      [ ] ARM errata: TLBI/DSB failure on Cortex-A15                                                                              < <  
  < <                                                      [ ] ARM errata: incorrect instructions may be executed from loop buffer                                                     < <  
  < <                                                      [ ] ARM errata: A12: some seqs of opposed cond code instrs => deadlock or corruption                                        < <  
  < <                                                      [ ] ARM errata: A12: sequence of VMOV to core registers might lead to a dead lock                                           < <  
  < <                                                      [ ] ARM errata: A12: DMB NSHST/ISHST mixed ... might cause deadlock                                                         < <  
  < <                                                      [ ] ARM errata: A12: CPU might deadlock under some very rare internal conditions                                            < <  
  < <                                                      [ ] ARM errata: A17: DMB ST might fail to create order between stores                                                       < <  
  < <                                                      [ ] ARM errata: A17: some seqs of opposed cond code instrs => deadlock or corruption                                        < <  
  < <                                                      [ ] ARM errata: A17: CPU might deadlock under some very rare internal conditions
  
  
kernel setup---------------------------------------------------------------------------------------------------------------------------------------------------------------------
  
  @^<                                                      [*] Symmetric Multi-Processing                                                                                              < <  
  < <                                                      [*]   Allow booting SMP kernel on uniprocessor systems                                                                      < <  
  < <                                                      [*]   Support cpu topology definition                                                                                       < <  
  < <                                                      [*]     Multi-core scheduler support                                                                                        < <  
  < <                                                      [*]     SMT scheduler support                                                                                               < <  
  < <                                                      [*] Architected timer support                                                                                               < <  
  < <                                                      [ ] Multi-Cluster Power Management                                                                                          < <  
  < <                                                      [ ] big.LITTLE support (Experimental)                                                                                       < <  
  < <                                                          Memory split (3G/1G user/kernel split)  --->                                                                            < <  
  < <                                                      (4) Maximum number of CPUs (2-32)                                                                                           < <  
  < <                                                      -*- Support for hot-pluggable CPUs                                                                                          < <  
  < <                                                      [ ] Support for the ARM Power State Coordination Interface (PSCI)                                                           < <  
  < <                                                          Timer frequency (100 Hz)  --->                                                                                          < <  
  < <                                                      [ ] Compile the kernel in Thumb-2 mode                                                                                      < <  
  < <                                                      [*] Runtime patch udiv/sdiv instructions into __aeabi_{u}idiv()                                                             < <  
  < <                                                      -*- Use the ARM EABI to compile the kernel                                                                                  < <  
  < <                                                      [ ]   Allow old ABI binaries to run with this kernel (EXPERIMENTAL)                                                         < <  
  < <                                                      [*] High Memory Support                                                                                                     < <  
  < <                                                      [*]   Allocate 2nd-level pagetables from highmem                                                                            < <  
  < <                                                      [*] Enable use of CPU domains to implement privileged no-access                                                             < <  
  < <                                                      [*] Use PLTs to allow module memory to spill over into vmalloc area                                                         < <  
  < <                                                      (11) Maximum zone order                                                                                                     < <  
  < <                                                      [ ] Use kernel mem{cpy,set}() for {copy_to,clear}_user()                                                                    < <  
  < <                                                      [ ] Enable seccomp to safely compute untrusted bytecode                                                                     < <  
  < <                                                      [ ] Enable paravirtualization code                                                                                          < <  
  < <                                                      [ ] Paravirtual steal time accounting                                                                                       < <  
  < <                                                      [ ] Xen guest support on ARM                                                                                                < <  
  < <                                                                                                                                                                                  < <  
  < <                                                                                                                                                                                  < <  
  < <                                                                                                                                                                                  < <  
  < <                                                                                                                                                                                  < <  
  < <                                                                                                                                                                                  < <  
  < <                                                                                                                                                                                  < <  
  < <                                                                                                                                                                                  < <  
  < <                                                                                                                                                                                  < <  
  < ^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^^@^@^@^@@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@  

Boot Options----------------------------------------------------------------------------------------------------------------------------------------------------------------

@^<                                                      -*- Flattened Device Tree support                                                                                           < <  
  < <                                                      [*]   Support for the traditional ATAGS boot data passing                                                                   < <  
  < <                                                      [ ]     Provide old way to pass kernel parameters                                                                           < <  
  < <                                                      (0x0) Compressed ROM boot loader base address                                                                               < <  
  < <                                                      (0x0) Compressed ROM boot loader BSS address                                                                                < <  
  < <                                                      [ ] Use appended device tree blob to zImage (EXPERIMENTAL)                                                                  < <  
  < <                                                      ()  Default kernel command string                                                                                           < <  
  < <                                                      [ ] Build kdump crash kernel (EXPERIMENTAL)                                                                                 < <  
  < <                                                      -*- Auto calculation of the decompressed kernel image address                                                               < <  
  < <                                                      [ ] UEFI runtime support                                                                                                    < <  
  
  CPU Power Management--------------------------------------------------------------
  
  @^<                                                          CPU Frequency scaling  --->                                                                                             < <  
  < <                                                          CPU Idle  --->                                                                                                          < <  
  
	               Cpu Frequency Scaling--------------------------------
				   
				                                                      [*] CPU Frequency scaling                                                                                                   < <  
  < <                                                      [ ]   CPU frequency transition statistics                                                                                   < <  
  < <                                                            Default CPUFreq governor (userspace)  --->                                                                            < <  
  < <                                                      <*>   'performance' governor                                                                                                < <  
  < <                                                      <*>   'powersave' governor                                                                                                  < <  
  < <                                                      -*-   'userspace' governor for userspace frequency scaling                                                                  < <  
  < <                                                      <*>   'ondemand' cpufreq policy governor                                                                                    < <  
  < <                                                      <*>   'conservative' cpufreq governor                                                                                       < <  
  < <                                                      [ ]   'schedutil' cpufreq policy governor                                                                                   < <  
  < <                                                            *** CPU frequency scaling drivers ***                                                                                 < <  
  < <                                                      < >   Generic DT based cpufreq driver                                                                                       < <  
  < <                                                      < >   Generic ARM big LITTLE CPUfreq driver                                                                                 < <  
  < <                                                      < >   CPU frequency scaling driver for Freescale QorIQ SoCs                                                                 < <  
  < <
  
	  CPU Idle----------------------------------------
                   [*] CPU idle PM support                                                                                                     < <  
  < <                                                      [ ]   Ladder governor (for periodic timer tick)                                                                             < <  
  < <                                                      -*-   Menu governor (for tickless system)                                                                                   < <  
  < <                                                      [ ]   Timer events oriented (TEO) governor (for tickless systems)                                                           < <  
  < <                                                            ARM CPU Idle Drivers  --->                                                                                            < <  
  
  Power Management --------------------------------------------------------------------------------------------------------------

^<                                                      [*] Suspend to RAM and standby                                                                                              < <  
  < <                                                      [ ]   Skip kernel's sys_sync() on suspend to RAM/standby (NEW)                                                              < <  
  < <                                                      [ ] Hibernation (aka 'suspend to disk')                                                                                     < <  
  < <                                                      [ ] Opportunistic sleep (NEW)                                                                                               < <  
  < <                                                      [ ] User space wakeup sources interface (NEW)                                                                               < <  
  < <                                                      -*- Device power management core functionality                                                                              < <  
  < <                                                      [ ]   Power Management Debug Support (NEW)                                                                                  < <  
  < <                                                      < > Advanced Power Management Emulation                                                                                     < <  
  < <                                                      [ ] Enable workqueue power-efficient mode by default (NEW)                                                                  < <  
  < <                                                      [ ] Energy Model for CPUs                                                                                                   < <  
  
  
  
  
  
  
