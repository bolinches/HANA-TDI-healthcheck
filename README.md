Main logic besides this tool:
- Keeping the JSON files updated via github with issues or other flows
- Not checking stuff that is already check elsewhere (hwcct, saptune , ...)
- Directions on what to check more? (TSO?, MTU, ...) Storage seems to be rather annoying as we have now 3 supported ways in production and is more than just the OS layer but the backend but I guess all can be discussed
- Feedback in general (Python is a new thing for me so expect not great coding in there)

To run the tool just the get the hoh.py and the jsons (git clone or whichever way) and run it. It does not use any python module that is not installed by default so should be fine (the output is color coded cannot be seen here), To run it you need to pass one argument XFS/NFS/ESS, depending on the storage used for HANA data and logs
```
Welcome to HANA OS Healthchecker (hoh) version 1.1

Please use https://github.com/bolinches/HANA-TDI-healthcheck to get latest versions and report issues about hoh.

The purpose of hoh is to supplement the official tools like HWCCT not to substitute them, always refer to official documentation from IBM, SuSE/RedHat, and SAP

You should always check your system with latest version of HWCCT as explained on SAP note:1943937 - Hardware Configuration Check Tool - Central Note

JSON files versions:
        supported OS:           0.1
        sysctl:                 1.0
        packages:               0.2
        ibm power packages:     0.1

This software comes with absolutely no warranty of any kind. Use it at your own risk

Do you want to continue? (y/n):
 ```

You can the agree or disagree to run hoh

As example output of a system:

```
Checking OS version

OK:  SUSE Linux Enterprise Server for SAP Applications 12 SP2 is a supported OS for this tool

Checking NTP status

OK: NTP is configured in this system
OK: Network time sync is activated in this system

Checking if saptune solution is set to HANA

2205917 - SAP HANA DB: Recommended OS settings for SLES 12 / SLES for SAP Applications 12 -
        KernelMMTransparentHugepage Expected: never
        KernelMMTransparentHugepage Actual  :
The parameters listed above have deviated from the specified SAP solution recommendations.

ERROR: saptune is *NOT* fully using the solution HANA

The following individual SAP notes recommendations are available via sapnote
Consider enabling ALL of them, including 2161991 as only sets NOOP as I/O scheduler

All notes (+ denotes manually enabled notes, * denotes notes enabled by solutions):
*       1275776 Linux: Preparing SLES for SAP environments
*       1557506 Linux paging improvements
*       1984787 SUSE LINUX Enterprise Server 12: Installation notes
        2161991 VMware vSphere (guest) configuration guidelines
*       2205917 SAP HANA DB: Recommended OS settings for SLES 12 / SLES for SAP Applications 12
        SAP_ASE SAP_Adaptive_Server_Enterprise
        SAP_BOBJ        SAP_Business_OBJects
+       SUSE-GUIDE-01   SLES 12 OS Tuning & Optimization Guide – Part 1
+       SUSE-GUIDE-02   SLES 12: Network, CPU Tuning and Optimization – Part 2

Checking sysctl settings:

OK: vm.pagecache_limit_mb it is set to the recommended value of 0
OK: net.core.somaxconn it is set to the recommended value of 4096
OK: net.ipv4.tcp_max_syn_backlog it is set to the recommended value of 8192
OK: net.ipv4.tcp_tw_reuse it is set to the recommended value of 1
OK: net.ipv4.tcp_timestamps it is set to the recommended value of 1
OK: net.ipv4.tcp_rmem it is set to the recommended value of 4096 16384 4194304
OK: net.ipv4.tcp_wmem it is set to the recommended value of 4096 16384 4194304
OK: net.ipv4.tcp_slow_start_after_idle it is set to the recommended value of 0
OK: net.ipv4.tcp_syn_retries it is set to the recommended value of 8
OK: net.ipv4.tcp_tw_recycle it is set to the recommended value of 1

Checking packages install status:

OK: ipmitool installation status is as expected
OK: powerpc-utils installation status is as expected
OK: pseries-energy installation status is as expected
OK: ibmPMLinux installation status is as expected
OK: ppc64-diag installation status is as expected

Checking IBM service and productivity tools packages install status:

WARNING: ibm-power-baremetal-sles12 installation status is *NOT* as expected. Check that at least one package is installed
OK: ibm-power-managed-sles12 installation status is installed
WARNING: ibm-power-nonmanaged-sles12 installation status is *NOT* as expected. Check that at least one package is installed
OK: ibm-power-kvmguest-sles12 installation status is not installed


The summary of this run:
time configurations reported no deviations
saptune reported deviations
sysctl reported no deviations
packages reported no deviations
IBM service and productivity tools packages reported no deviations
```

NOTE: There is a bug on this saptune version that looks for KernelMMTransparentHugepage on Power, hence the deviation above
