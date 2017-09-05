+++
title = "The Linux Kernel Configuration Guide Part 7"
slug = "the linux kernel configuration guide part 7"
nick = "kernel6"
date = "2017-09-06"
author = "Firas Khalil Khana"
imgsrc = "/img/kernel7.png"
imgalt = "kernel7"
categories = [ "kernel" ]
+++
<h3>Power management and ACPI options  ---></h3>
<h3>[ ] Suspend to RAM and standby</h3>
```none
Symbol:     CONFIG_SUSPEND

Help:       Allow the system to enter sleep states in which main memory is
            powered and thus its contents are preserved, such as the
            suspend-to-RAM state (e.g. the ACPI S3 state).

Type:       boolean

Choice:     excluded [ ]

Reason:     I excluded this option as I never use suspend to RAM.
```
<h3>[ ] Hibernation (aka 'suspend to disk')</h3>
```none
Symbol:     CONFIG_HIBERNATION

Help:       Enable the suspend to disk (STD) functionality, which is usually
            called "hibernation" in user interfaces.  STD checkpoints the
            system and powers it off; and restores that checkpoint on reboot.

            You can suspend your machine with 'echo disk > /sys/power/state'
            after placing resume=/dev/swappartition on the kernel command line
            in your bootloader's configuration file.

            Alternatively, you can use the additional userland tools available
            from <http://suspend.sf.net>.

            In principle it does not require ACPI or APM, although for example
            ACPI will be used for the final steps when it is available.  One
            of the reasons to use software suspend is that the firmware hooks
            for suspend states like suspend-to-RAM (STR) often don't work very
            well with Linux.

            It creates an image which is saved in your active swap. Upon the next
            boot, pass the 'resume=/dev/swappartition' argument to the kernel to
            have it detect the saved image, restore memory state from it, and
            continue to run as before. If you do not want the previous state to
            be reloaded, then use the 'noresume' kernel command line argument.
            Note, however, that fsck will be run on your filesystems and you will
            need to run mkswap against the swap partition used for the suspend.

            It also works with swap files to a limited extent (for details see
            <file:Documentation/power/swsusp-and-swap-files.txt>).

            Right now you may boot without resuming and resume later but in the
            meantime you cannot use the swap partition(s)/file(s) involved in
            suspending.  Also in this case you must not use the filesystems
            that were mounted before the suspend.  In particular, you MUST NOT
            MOUNT any journaled filesystems mounted before the suspend or they
            will get corrupted in a nasty way.

            For more information take a look at <file:Documentation/power/swsusp.txt>.

Type:       boolean

Choice:     excluded [ ]

Reason:     I excluded this option as I never use hibernation.

            CONFIG_HIBERNATION or suspend to disk saves all data in the memory to
            the disk allowing the device to power off completely.
```
<h3>[&ast;] Device power management core functionality</h3>
```none
Symbol:     CONFIG_PM

Help:       Enable functionality allowing I/O devices to be put into energy-saving
            (low power) states, for example after a specified period of inactivity
            (autosuspended), and woken up in response to a hardware-generated
            wake-up event or a driver's request.

            Hardware support is generally required for this functionality to work
            and the bus type drivers of the buses the devices are on are
            responsible for the actual handling of device suspend requests and
            wake-up events.

Type:       boolean

Choice:     built-in [*]

Reason:     I (and the Gentoo Wiki) highly recommend that you include this option
            in your kernel if powersaving is one of your concerns.
```
<h3>[ ]   Power Management Debug Support</h3>
```none
Symbol:     CONFIG_PM_DEBUG

Help:       This option enables various debugging support in the Power Management
            code. This is helpful when debugging and reporting PM bugs, like
            suspend support.

Type:       boolean

Choice:     excluded [ ]

Reason:     I have no use for this debugging option; thus, I excluded it.

            If you're facing trouble getting your suspend suppor to work then
            including this option might help you debug the problem.
```
<h3>[ ] Enable workqueue power-efficient mode by default</h3>
```none
Symbol:     CONFIG_WQ_POWER_EFFICIENT_DEFAULT

Help:       Per-cpu workqueues are generally preferred because they show
            better performance thanks to cache locality; unfortunately,
            per-cpu workqueues tend to be more power hungry than unbound
            workqueues.

            Enabling workqueue.power_efficient kernel parameter makes the
            per-cpu workqueues which were observed to contribute
            significantly to power consumption unbound, leading to measurably
            lower power usage at the cost of small performance overhead.

            This config option determines whether workqueue.power_efficient
            is enabled by default.

            If in doubt, say N.

Type:       boolean

Choice:     excluded [ ]

Reason:     I excluded this option as it reduces performance (by a tiny bit).
            
            You should include this option is powersaving is one of your concerns.
```
<h3>[&ast;] ACPI (Advanced Configuration and Power Interface) Support  ---></h3>
```none
Symbol:     CONFIG_ACPI

Help:       Advanced Configuration and Power Interface (ACPI) support for
            Linux requires an ACPI-compliant platform (hardware/firmware),
            and assumes the presence of OS-directed configuration and power
            management (OSPM) software.  This option will enlarge your
            kernel by about 70K.

            Linux ACPI provides a robust functional replacement for several
            legacy configuration and power management interfaces, including
            the Plug-and-Play BIOS specification (PnP BIOS), the
            MultiProcessor Specification (MPS), and the Advanced Power
            Management (APM) specification.  If both ACPI and APM support
            are configured, ACPI is used.

            The project home page for the Linux ACPI subsystem is here:
            <https://01.org/linux-acpi>

            Linux support for ACPI is based on Intel Corporation's ACPI
            Component Architecture (ACPI CA).  For more information on the
            ACPI CA, see:
            <http://acpica.org/>

            ACPI is an open industry specification originally co-developed by
            Hewlett-Packard, Intel, Microsoft, Phoenix, and Toshiba. Currently,
            it is developed by the ACPI Specification Working Group (ASWG) under
            the UEFI Forum and any UEFI member can join the ASWG and contribute
            to the ACPI specification.
            The specification is available at:
            <http://www.acpi.info>
            <http://www.uefi.org/acpi/specs>

Type:       boolean

Choice:     built-in [*]

Reason:     ACPI enforces strict guidelines on devices to significantly increase
            powersaving.

            Not all devices follow or abide by these guidelines which may lead to
            several warnings,errors and conflicts. (Most of the times they can be
            safely ignored.)

            I included this option as my laptop heavily relies on ACPI (even though
            I get a ton of harmless warnings in my dmesg output).
```
<h3>[ ]   AML debugger interface</h3>
```none
Symbol:     CONFIG_ACPI_DEBUGGER

Help:       Enable in-kernel debugging of AML facilities: statistics,
            internal object dump, single step control method execution.
            This is still under development, currently enabling this only
            results in the compilation of the ACPICA debugger files.

Type:       boolean

Choice:     excluded [ ]

Reason:     I excluded this option as I have no need for this debugging option.
```
<h3>[ ]   Deprecated power /proc/acpi directories</h3>
```none
Symbol:     CONFIG_ACPI_PROCFS_POWER

Help:       For backwards compatibility, this option allows
            deprecated power /proc/acpi/ directories to exist, even when
            they have been replaced by functions in /sys.
            The deprecated directories (and their replacements) include:
            /proc/acpi/battery/* (/sys/class/power_supply/*)
            /proc/acpi/ac_adapter/* (sys/class/power_supply/*)
            This option has no effect on /proc/acpi/ directories
            and functions, which do not yet exist in /sys
            This option, together with the proc directories, will be
            deleted in the future.

            Say N to delete power /proc/acpi/ directories that have moved to /sys/

Type:       boolean

Choice:     excluded [ ]

Reason:     I normally tend to exclude deprecated options (unless really needed).

            If your reporting tools rely on this option and/or you were receiving 
            some ACPI warnings/errors on your system, then including this option 
            may help solve it.
```
<h3>[ ]   Allow supported ACPI revision to be overridden</h3>
```none
Symbol:     CONFIG_ACPI_REV_OVERRIDE_POSSIBLE

Help:       The platform firmware on some systems expects Linux to return "5" as
            the supported ACPI revision which makes it expose system configuration
            information in a special way.

            For example, based on what ACPI exports as the supported revision,
            Dell XPS 13 (2015) configures its audio device to either work in HDA
            mode or in I2S mode, where the former is supposed to be used on Linux
            until the latter is fully supported (in the kernel as well as in user
            space).

            This option enables a DMI-based quirk for the above Dell machine (so
            that HDA audio is exposed by the platform firmware to the kernel) and
            makes it possible to force the kernel to return "5" as the supported
            ACPI revision via the "acpi_rev_override" command line switch.

Type:       boolean

Choice:     excluded [ ]

Reason:     I excluded this option as I have no need for it on my system.
```
<h3>< >   EC read/write access through /sys/kernel/debug/ec</h3>
```none
Symbol:     CONFIG_ACPI_EC_DEBUGFS

Help:       Say N to disable Embedded Controller /sys/kernel/debug interface

            Be aware that using this interface can confuse your Embedded
            Controller in a way that a normal reboot is not enough. You then
            have to power off your system, and remove the laptop battery for
            some seconds.
            An Embedded Controller typically is available on laptops and reads
            sensor values like battery state and temperature.
            The kernel accesses the EC through ACPI parsed code provided by BIOS
            tables. This option allows to access the EC directly without ACPI
            code being involved.
            Thus this option is a debug option that helps to write ACPI drivers
            and can be used to identify ACPI code or EC firmware bugs.

Type:       tristate

Choice:     excluded < >

Reason:     Usually ECs are found in laptops to read info provided by sensors,
            this option allows the linux kernel to access the EC through ACPI

            However, when using this option, the kernel can access the EC without
            ACPI which sounded problematic to me, so I excluded it.
```
<h3><&ast;>   AC Adapter</h3>
```none
Symbol:     CONFIG_ACPI_AC

Help:       This driver supports the AC Adapter object, which indicates
            whether a system is on AC or not.  If you have a system that can
            switch between A/C and battery, say Y.

            To compile this driver as a module, choose M here:
            the module will be called ac.

Type:       tristate

Choice:     built-in <*>

Reason:     I'm building the kernel for my laptop which switches between A/C and battery
            so yeah I need this option.

            You can safely exclude this option along with CONFIG_ACPI_BATTERY if
            you're building the kernel for a desktop.
```
<h3><&ast;>   Battery</h3>
```none
Symbol:     CONFIG_ACPI_BATTERY

Help:       This driver adds support for battery information through
            /proc/acpi/battery. If you have a mobile system with a battery,
            say Y.

            To compile this driver as a module, choose M here:
            the module will be called battery.

Type:       tristate

Choice:     built-in <*>

Reason:     I'm building the kernel for my laptop which switches between A/C and battery
            so I included this option.

            You can safely exclude this option along with CONFIG_ACPI_AC if you're
            building the kernel for a desktop.
```
<h3>-&ast;-   Button</h3>
```none
Symbol:     CONFIG_ACPI_BUTTON

Help:       This driver handles events on the power, sleep, and lid buttons.
            A daemon reads events from input devices or via netlink and
            performs user-defined actions such as shutting down the system.
            This is necessary for software-controlled poweroff.

            To compile this driver as a module, choose M here:
            the module will be called button.

Type:       tristate

Choice:     built-in -*-

Reason:     Forcibly included on my system as it's required by some important options.
            
            It's generally a good idea to include this option in your kernel.
```
<h3>-&ast;-   Video</h3>
```none
Symbol:     CONFIG_ACPI_VIDEO

Help:       This driver implements the ACPI Extensions For Display Adapters
            for integrated graphics devices on motherboard, as specified in
            ACPI 2.0 Specification, Appendix B.  This supports basic operations
            such as defining the video POST device, retrieving EDID information,
            and setting up a video output.

            To compile this driver as a module, choose M here:
            the module will be called video.

Type:       tristate

Choice:     built-in -*-

Reason:     Forcibly included on my system as it's required by a ton of other options.

            It's generally a good idea to include this option in your kernel.
```
<h3><&ast;>   Fan</h3>
```none
Symbol:     CONFIG_ACPI_FAN

Help:       This driver supports ACPI fan devices, allowing user-mode
            applications to perform basic fan control (on, off, status).

            To compile this driver as a module, choose M here:
            the module will be called fan.

Type:       tristate

Choice:     built-in -*-

Reason:     It's highly recommended that you include this option in your kernel.
```
<h3>[ ]   Dock</h3>
```none
Symbol:     CONFIG_ACPI_DOCK

Help:       This driver supports ACPI-controlled docking stations and removable
            drive bays such as the IBM Ultrabay and the Dell Module Bay.

Type:       boolean

Choice:     excluded [ ]

Reason:     I excluded this option as my laptop doesn't have dock or a removable
            drive bay.
```
<h3>-&ast;-   Processor</h3>
```none
Symbol:     CONFIG_ACPI_PROCESSOR

Help:       This driver adds support for the ACPI Processor package. It is required
            by several flavors of cpufreq performance-state, thermal, throttling and
            idle drivers.

            To compile this driver as a module, choose M here:
            the module will be called processor.

Type:       tristate

Choice:     built-in -*-

Reason:     A must have option for cpu frequency scaling.

            Intel P-State the relatively new cpu frequency scaling driver requires it.
```
<h3><&ast;>   Processor Aggregator</h3>
```none
Symbol:     CONFIG_ACPI_PROCESSOR_AGGREGATOR

Help:       ACPI 4.0 defines processor Aggregator, which enables OS to perform
            specific processor configuration and control that applies to all
            processors in the platform. Currently only logical processor idling
            is defined, which is to reduce power consumption. This driver
            supports the new device.

Type:       tristate

Choice:     built-in <*>

Reason:     Allows the kernel to let the CPUs enter idle state, which is a good
            idea; hence, I included this option.
```
<h3><&ast;>   Thermal Zone</h3>
```none
Symbol:     CONFIG_ACPI_THERMAL

Help:       This driver supports ACPI thermal zones.  Most mobile and
            some desktop systems support ACPI thermal zones.  It is HIGHLY
            recommended that this option be enabled, as your processor(s)
            may be damaged without it.

            To compile this driver as a module, choose M here:
            the module will be called thermal.

Type:       tristate

Choice:     Wow... processors may be damaged without it... wondering why this
            option isn't forcibly included but anyways you must include this
            option in your kernel.

            It allows the kernel along with CONFIG_ACPI_FAN to manage the fan's
            speed; thus, lowering the system's temperature.
```
<h3>()    Custom DSDT Table file to include</h3>
```none
Symbol:     CONFIG_ACPI_CUSTOM_DSDT_FILE

Help:       This option supports a custom DSDT by linking it into the kernel.
            See Documentation/acpi/dsdt-override.txt

            Enter the full path name to the file which includes the AmlCode
            declaration.

            If unsure, don't enter a file name.

Type:       string

Choice:     () default

Reason:     I went with the default value for this option as this option is mostly
            for developers or users who are willing to use a custom DSDT table mainly
            for fixing some ACPI errors (this is really an advanced complex process).
            For more info:
                
                https://forums.gentoo.org/viewtopic.php?t=122145
```
<h3>[ ]   Debug Statements</h3>
```none
Symbol:     CONFIG_ACPI_DEBUG

Help:       The ACPI subsystem can produce debug output.  Saying Y enables this
            output and increases the kernel size by around 50K.

            Use the acpi.debug_layer and acpi.debug_level kernel command-line
            parameters documented in Documentation/acpi/debug.txt and
            Documentation/admin-guide/kernel-parameters.rst to control the type and
            amount of debug output.

Type:       boolean

Choice:     excluded [ ]

Reason:     I excluded this option as I've fixed most ACPI errors on my system (still
            getting some harmless warnings though).

            Include this option if you want to debug ACPI errors.
```
<h3>[ ]   PCI slot detection driver</h3>
```none
Symbol:     CONFIG_ACPI_PCI_SLOT

Help:       This driver creates entries in /sys/bus/pci/slots/ for all PCI
            slots in the system.  This can help correlate PCI bus addresses,
            i.e., segment/bus/device/function tuples, with physical slots in
            the system.  If you are unsure, say N.

Type:       boolean

Choice:     excluded [ ]

Reason:     I went with the default value as well for this option.

            Some applications may complain about this missing option. If that
            ever happens include this option.
```
<h3>[&ast;]   Power Management Timer Support</h3>
```none
Symbol:     CONFIG_X86_PM_TIMER

Help:       The Power Management Timer is available on all ACPI-capable,
            in most cases even if ACPI is unusable or blacklisted.

            This timing source is not affected by power management features
            like aggressive processor idling, throttling, frequency and/or
            voltage scaling, unlike the commonly used Time Stamp Counter
            (TSC) timing source.

            You should nearly always say Y here because many modern
            systems require this timer.

Type:       boolean

Choice:     built-in [*]

Reason:     It's highly recommended that you include this option in your kernel.
```
<h3>-&ast;-   Container and Module Devices</h3>
```none
Symbol:     CONFIG_ACPI_CONTAINER

Help:       This driver supports ACPI Container and Module devices (IDs
            ACPI0004, PNP0A05, and PNP0A06).

            This helps support hotplug of nodes, CPUs, and memory.

            To compile this driver as a module, choose M here:
            the module will be called container.

Type:       boolean

Choice:     built-in -*-

Reason:     Forcibly included as it's required by several important options.
```
<h3><&ast;>   Smart Battery System</h3>
```none
Symbol:     CONFIG_ACPI_SBS

Help:       This driver supports the Smart Battery System, another
            type of access to battery information, found on some laptops.

            To compile this driver as a module, choose M here:
            the modules will be called sbs and sbshc.

Type:       tristate

Choice:     built-in <*>

Reason:     Although I suspect the existence of a smart battery system on my system
            I included this option. (I'll update this option once I have more info)
```
<h3>< >   Hardware Error Device</h3>
```none
Symbol:     CONFIG_ACPI_HED

Help:       This driver supports the Hardware Error Device (PNP0C33),
            which is used to report some hardware errors notified via
            SCI, mainly the corrected errors.

Type:       tristate

Choice:     excluded [ ]

Reason:     I excluded this option as I don't have a (PNP0C33) hardware error device.

            Even for users that have this hardware error device, the reports are mostly
            about corrected errors only and not all errors.
```
<h3>[ ]   Hardware-reduced ACPI support only</h3>
```none
Symbol:     CONFIG_ACPI_REDUCED_HARDWARE_ONLY

Help:       This config item changes the way the ACPI code is built.  When this
            option is selected, the kernel will use a specialized version of
            ACPICA that ONLY supports the ACPI "reduced hardware" mode.  The
            resulting kernel will be smaller but it will also be restricted to
            running in ACPI reduced hardware mode ONLY.

            If you are unsure what to do, do not enable this option.

Type:       boolean

Choice:     excluded [ ]

Reason:     This option is mostly for SoC and arm based devices; therefore, I 
            excluded it.
```
<h3>< >   ACPI NVDIMM Firmware Interface Table (NFIT)</h3>
```none
Symbol:     CONFIG_ACPI_NFIT

Help:       Infrastructure to probe ACPI 6 compliant platforms for
            NVDIMMs (NFIT) and register a libnvdimm device tree.  In
            addition to storage devices this also enables libnvdimm to pass
            ACPI._DSM messages for platform/dimm configuration.

            To compile this driver as a module, choose M here:
            the module will be called nfit.

Type:       tristate

Choice:     excluded < >

Reason:     I excluded this option as I don't have NVDIMMs installed in my 
            system.
```
<h3>[ ]   ACPI Platform Error Interface (APEI)</h3>
```none
Symbol:     CONFIG_ACPI_APEI

Help:       APEI allows to report errors (for example from the chipset)
            to the operating system. This improves NMI handling
            especially. In addition it supports error serialization and
            error injection.

Type:       boolean

Choice:     excluded [ ]

Reason:     I excluded this option as I saw no need for it on my system.

            If you think I should include this option in my kernel then
            kindly post a comment below or send me an email explaining
            why.
```
<h3>< >   DPTF Platform Power Participant</h3>
```none
Symbol:     CONFIG_DPTF_POWER

Help:       This driver adds support for Dynamic Platform and Thermal Framework
            (DPTF) Platform Power Participant device (INT3407) support.
            This participant is responsible for exposing platform telemetry:
                  max_platform_power
                  platform_power_source
                  adapter_rating
                  battery_steady_power
                  charger_type

            To compile this driver as a module, choose M here:
            the module will be called dptf_power.

Type:       tristate

Choice:     excluded < >

Reason:     I excluded this option as my system doesn't need it.
```
<h3>< >   Extended Error Log support</h3>
```none
Symbol:     CONFIG_ACPI_EXTLOG

Help:       Certain usages such as Predictive Failure Analysis (PFA) require
            more information about the error than what can be described in
            processor machine check banks. Most server processors log
            additional information about the error in processor uncore
            registers. Since the addresses and layout of these registers vary
            widely from one processor to another, system software cannot
            readily make use of them. To complicate matters further, some of
            the additional error information cannot be constructed without
            detailed knowledge about platform topology.

            Enhanced MCA Logging allows firmware to provide additional error
            information to system software, synchronous with MCE or CMCI. This
            driver adds support for that functionality with corresponding
            tracepoint which carries that information to userspace.

Type:       tristate

Choice:     excluded < >

Reason:     I excluded this option as I have no need for the extended log information.

            If you're suffering from such complicated errors as described in Help then
            include this option, otherwise safely exclude it.
```
<h3>[ ]   PMIC (Power Management Integrated Circuit) operation region support  ----</h3>
```none
Symbol:     CONFIG_PMIC_OPREGION

Help:       Select this option to enable support for ACPI operation
            region of the PMIC chip. The operation region can be used
            to control power rails and sensor reading/writing on the
            PMIC chip.

Type:       boolean

Choice:     excluded [ ]

Reason:     I excluded this option as I don't have a PMIC chip in my system.
```
<h3>< >   ACPI configfs support</h3>
```none
Symbol:     CONFIG_ACPI_CONFIGFS

Help:       Select this option to enable support for ACPI configuration from
            userspace. The configurable ACPI groups will be visible under
            /config/acpi, assuming configfs is mounted under /config.

Type:       tristate

Choice:     excluded < >

Reason:     I excluded this option as I have no need for it.
```
<h3>[ ] SFI (Simple Firmware Interface) Support  ----</h3>
```none
Symbol:     CONFIG_SFI

Help:       The Simple Firmware Interface (SFI) provides a lightweight method
            for platform firmware to pass information to the operating system
            via static tables in memory.  Kernel SFI support is required to
            boot on SFI-only platforms.  Currently, all SFI-only platforms are
            based on the 2nd generation Intel Atom processor platform,
            code-named Moorestown.

            For more information, see http://simplefirmware.org

            Say 'Y' here to enable the kernel to boot on SFI-only platforms.

Type:       boolean

Choice:     excluded [ ]

Reason:     This option is intended for small and low speced devices (mobile
            devices); therefore, I excluded it.
```
<h3>CPU Frequency scaling  ---></h3>
<h3>[&ast;] CPU Frequency scaling</h3>
```none
Symbol:     CONFIG_CPU_FREQ

Help:       CPU Frequency scaling allows you to change the clock speed of
            CPUs on the fly. This is a nice method to save power, because
            the lower the CPU clock speed, the less power the CPU consumes.

            Note that this driver doesn't automatically change the CPU
            clock speed, you need to either enable a dynamic cpufreq governor
            (see below) after boot, or use a userspace tool.

            For details, take a look at <file:Documentation/cpu-freq>.

            If in doubt, say N.

Type:       boolean

Choice:     built-in [*]

Reason:     Another important option that should be included (especially if
            you're on a laptop or want to conserve power) as it'll scale down
            your CPU's clock speed when it's not being use to conserve power.
```
<h3>[ ]   CPU frequency transition statistics</h3>
```none
Symbol:     CONFIG_CPU_FREQ_STAT

Help:       Export CPU frequency statistics information through sysfs.
            
            If in doubt, say N.

Type:       boolean

Choice:     excluded [ ]

Reason:     I excluded this option as I have no need for the statistical information
            it provides for less system overhead.
```
<h3>Default CPUFreq governor (ondemand)  ---></h3>
```none
Help:       This option sets which CPUFreq governor shall be loaded at
            startup. If in doubt, select 'performance'.
```
<h3>(X) ondemand</h3>
```none
Symbol:     CONFIG_CPU_FREQ_DEFAULT_GOV_ONDEMAND

Help:       Use the CPUFreq governor 'ondemand' as default. This allows
            you to get a full dynamic frequency capable system by simply
            loading your cpufreq low-level hardware driver.
            Be aware that not all cpufreq drivers support the ondemand
            governor. If unsure have a look at the help section of the
            driver. Fallback governor will be the performance governor.

Type:       boolean

Choice:     built-in (X)

Reason:     Using a modern intel CPU (mine is from gen. 4), and in order to enable
            proper CPU scaling, you need to use CONFIG_X86_INTEL_PSTATE.

            However, the wiki says that when you're using the Intel P State frequency
            scaling driver you have to use the 'performance' governor. The result is
            that your CPUs will be always running at maximum frequency even on idle
            and will consume more energy and increase temperatures (when it's not needed).

            The correct way would be to use 'ondemand' as the default governor, while
            enabling both 'performance' and 'powersave' and use Intel P State as the 
            CPU frequency driver and exclude every other governor and CPU frequency
            scaling driver in order to get proper CPU scaling working.

            This way your CPUs will clock down to their minimum frequency when idle and
            not in use, and will max their frequency when on load which allows for
            reducing temperatures and conserving power while maintaining high performance.
```
<h3>-&ast;-   'performance' governor</h3>
```none
Symbol:     CONFIG_CPU_FREQ_GOV_PERFORMANCE

Help:       This cpufreq governor sets the frequency statically to the
            highest available CPU frequency.

            To compile this driver as a module, choose M here: the
            module will be called cpufreq_performance.

            If in doubt, say Y.

Type:       tristate

Choice:     built-in -*-

Reason:     Required to get the Intel P State CPU frequency scaling driver to work
            correctly.
```
<h3><&ast;>   'powersave' governor</h3>
```none
Symbol:     CONFIG_CPU_FREQ_GOV_POWERSAVE

Help:       This cpufreq governor sets the frequency statically to the
            lowest available CPU frequency.

            To compile this driver as a module, choose M here: the
            module will be called cpufreq_powersave.

            If in doubt, say Y.

Type:       tristate

Choice:     built-in <*>

Reason:     Required to allow the 'ondemand' governor when using the Intel P State
            CPU frequency driver to clock down the CPU's frequency to the lowest
            available frequency to conserve energy when in idle state.
```
<h3>-&ast;-   'ondemand' cpufreq policy governor</h3>
```none
Symbol:     CONFIG_CPU_FREQ_GOV_ONDEMAND

Help:       'ondemand' - This driver adds a dynamic cpufreq policy governor.
            The governor does a periodic polling and
            changes frequency based on the CPU utilization.
            The support for this governor depends on CPU capability to
            do fast frequency switching (i.e, very low latency frequency
            transitions).

            To compile this driver as a module, choose M here: the
            module will be called cpufreq_ondemand.

            For details, take a look at linux/Documentation/cpu-freq.

            If in doubt, say N.

Type:       tristate

Choice:     built-in -*-

Reason:     The correct governor that should be used with Intel P State.

            Remember that you need to choose 'ondemand' as the default governor 
            while enabling 'performance' and 'powersave' in order for Intel P State 
            driver to work correctly.

            Don't forget to disable every other governor ('userspace', 'conservative'
            and 'schedutil')as well for a more minimal system, as they won't be used.
```
<h3>&ast;&ast;&ast; CPU frequency scaling drivers &ast;&ast;&ast;</h3>
<h3>[&ast;]   Intel P state control</h3>
```none
Symbol:     CONFIG_X86_INTEL_PSTATE

Help:       This driver provides a P state for Intel core processors.
            The driver implements an internal governor and will become
            the scaling driver and governor for Sandy bridge processors.

            When this driver is enabled it will become the preferred
            scaling driver for Sandy bridge processors.

            If in doubt, say N.

Type:       boolean

Choice:     built-in

Reason:     If you're using a gen 2+ intel cpu then you definitely want this
            CPU frequency driver included.

            Dont' forget to include CONFIG_CPU_FREQ_GOV_ONDEMAND as your default
            governor and enable both CONFIG_CPU_FREQ_GOV_PERFORMANCE and
            CONFIG_CPU_FREQ_GOV_POWERSAVE and exclude every other governor and
            CPU scaling driver for a more minimal system as they won't be used.
```
<h3>CPU Idle  ---></h3>
<h3>-&ast;- CPU idle PM support</h3>
```none
Symbol:     CONFIG_CPU_IDLE

Help:       CPU idle is a generic framework for supporting software-controlled
            idle processor power management.  It includes modular cross-platform
            governors that can be swapped during runtime.

            If you're using an ACPI-enabled platform, you should say Y here.

Type:       boolean

Choice:     built-in -*-

Reason:     I highly recommend that you include this option as it allows your CPU
            to enter idle state when not in use to conserve energy.
```
<h3>-&ast;-   Ladder governor (for periodic timer tick)</h3>
```none
Symbol:     CONFIG_CPU_IDLE_GOV_LADDER

Help:       There is no help available for this option.

Type:       boolean

Choice:     built-in -*-

Reason:     Forcibly included as I have CONFIG_HZ_PERIODIC included as well.
            
            Exclude this option if you've included CONFIG_NO_HZ and
            CONFIG_NO_HZ_FULL.
```
<h3>[ ]   Menu governor (for tickless system)</h3>
```none
Symbol:     CONFIG_CPU_IDLE_GOV_MENU

Help:       There is no help available for this option.

Type:       boolean

Choice:     excluded [ ]

Reason:     I excluded this option as I have CONFIG_HZ_PERIODIC.

            Include this option if you've included CONFIG_NO_HZ and
            CONFIG_NO_HZ_FULL (that is if this option isn't forcibly
            included already).
```
<h3>[&ast;] Cpuidle Driver for Intel Processors</h3>
```none
Symbol:     CONFIG_INTEL_IDLE

Help:       Enable intel_idle, a cpuidle driver that includes knowledge of
            native Intel hardware idle features.  The acpi_idle driver
            can be configured at the same time, in order to handle
            processors intel_idle does not support.

Type:       boolean

Choice:     built-in [*]

Reason:     If you're using an intel cpu, you'd want to include this option
            in your kernel along with CONFIG_CPU_IDLE as it's much more
            compatible with your intel cpu and should facilitate the process
            of idling.
```