
> There's now a [new version of the Intel Compute stick](http://linuxgizmos.com/latest-intel-compute-sticks-use-skylake-and-cherry-trail-cpus/) out, 
> which may render working on this one moot (except for the fact that I hate to see computer hardware going to waste).  Certainly, 
> things are going to get more difficult to find on the web.


### WOOHOO !
{% endhighlight %}


#### Worth watching : 

*   Complete freeze on Intel Compute Stick, sometimes IRQ disabled #33 
    *   https://github.com/hadess/rtl8723bs/issues/33



### Getting the HDMI Audio going

...




------------------

The following issue on the rtl8723bs github page makes more sense, after delving into audio issues (since it also mentions i915) :
*   https://github.com/hadess/rtl8723bs/issues/33


i915-baytrail issues ::
  https://lists.debian.org/debian-devel/2015/09/msg00553.html
    http://intel.archive.canonical.com/pool/public/o/oem-audio-i915-baytrail-dkms/
  https://lists.debian.org/debian-devel/2015/09/msg00560.html 
    :: points out that kernel may be quite new
    After cross checking the missing label / function / file etc in lxr website, the code confirmed have been changed from 3.16 --> 3.18 in large extend.  
    The i915 folder from the DKMS source package can be replaced with the Linux kernel source one (/drivers/gpu/drm/i915), almost all errors gone.
    The only error left is the missing reference from hdmi_audio_if.h which was actually backported from intel gma source code from kernel 2.6.x.
    I think the effort to port kernel 2.6.x source code to 3.18 is overwhelming.  So better stay at 3.16.

  https://wiki.edubuntu.org/Audio/i915
    3.16 and forward : Standard driver (i915) 

  http://mailman.alsa-project.org/pipermail/alsa-devel/2015-July/094335.html
    Here's what a good boot of baytrail-pcm-audio looks like : 
        [   15.552097] byt-rt5640 byt-rt5640: ASoC: CPU DAI baytrail-pcm-audio not registered
        [   15.632979] (NULL device *): FW version: 04.05.13.a0
        [   15.632986] (NULL device *): Build type: a0
        [   15.632990] (NULL device *): Build date: Apr  2 2014 14:14:39
        [   15.691461] byt-rt5640 byt-rt5640: rt5640-aif1 <-> baytrail-pcm-audio mapping ok
    Firmware options :
      http://mailman.alsa-project.org/pipermail/alsa-devel/2015-July/094401.html    
    
    Can you try the bytcr_rt5640 machine driver as the byt-rt5640 machine
    driver will be getting deprecated (since it only works with a small
    number of FWs). The bytcr_rt5640 driver works with a wider range of FW.
    
    Liam Girdwood <liam.r.girdwood at intel.com> wrote:
      When testing the bytcr_rt5640 machine driver give also a try to the latest firmwares:
      https://git.kernel.org/cgit/linux/kernel/git/vkoul/firmware.git/tree/intel?h=byt
      I had some success with fw_sst_0f28_ssp0.bin

    snd-soc-sst-bytcr-rt5640 will be loaded automatically if you use snd-intel-sst-acpi instead of snd-soc-sst-acpi, 
    you can blacklist the latter using a file in /etc/modprobe.d (look up the details), 
    or just remove snd-soc-sst-acpi.ko from the installation dir as a dirty hack.

    Note that different drivers expect different firmwares, you cannot mix them:
      snd-soc-sst-acpi   -> fw_sst_0f28.bin-48kHz_i2s_master
      snd-intel-sst-acpi -> fw_sst_0f28.bin
      snd-intel-sst-acpi -> fw_sst_0f28_ssp0.bin (renamed to fw_sst_0f28.bin)

    http://mailman.alsa-project.org/pipermail/alsa-devel/2015-July/094428.html
      Very thorough writing from a user (or two) with Vinod@intel
      
        This is dpcm enabled driver.  So please route the FE to BE and it should work...

        Nicolas, this can be done by using the commands from here:
        http://mailman.alsa-project.org/pipermail/alsa-devel/2015-June/094080.html

        In particular:
        amixer -c0 sset 'codec_out0 mix 0 pcm0_in' on
        amixer -c0 sset 'media0_out mix 0 media1_in' on

        $ cat /proc/interrupts | grep sst    
        7:          0          0          0          0   IO-APIC 28-fasteoi   intel_sst_driver
      
        Sorry I should have been clearer. The dynamic debug allows you to enbale debug logs without recompiling

        So after boot:
        echo -n 'module snd_soc_sst_mfld_platform +p' > /sys/kernel/debug/dynamic_debug/control

        this will enable prints, so when you start audio, the firmware would be
        downloaded, and please grab the dmesg ouput and send...

        A final note, you also need to set the right codec controls to get audio; 
        setting the FE-BE path is necessary to make playback "work" but it may not be enough to actually get sound, 
        so make sure you run the full script Vinod provided in :
        http://mailman.alsa-project.org/pipermail/alsa-devel/2015-June/094080.html

        http://mailman.alsa-project.org/pipermail/alsa-devel/2015-July/094663.html
          Ah, sorry, the info from that bug report is outdated, for the snd-intel-sst-acpi you have to change
            sound/soc/intel/atom/sst/sst_acpi.c:
            
            
        
Download from https://www.happyassassin.net/fedlet/repo/SRPMS/  ::
  mv ~/Downloads/kernel-4.2.0-0.rc6.git0.1.1awb.src.rpm .
  mv ~/Downloads/baytrail-firmware-1.2-1awb.src.rpm .

  within the rpm2cpio kernel, we have patch-XXX.xz
    mkdir kernel
    cd kernel
    rpm2cpio ../kernel-4.2.0-0.rc6.git0.1.1awb.src.rpm | cpio -idmv
    unxz patch-4.2-rc6.xz 
    grep -i baytrail patch-4.2-rc6 # ::
  
      [andrewsm@square kernel]$ grep -i baytrail patch-4.2-rc6 
      -	 * HPET on current version of Baytrail platform has accuracy
      +	 * HPET on the current version of the Baytrail platform has accuracy
      +/* Shift bits for getting status for Valley View/Baytrail display */
      +	 * this power. For other platforms, like Baytrail/Braswell, only the
      @@ -7,4 +7,4 @@ obj-$(CONFIG_SND_SOC_INTEL_BAYTRAIL) += baytrail/
      diff --git a/sound/soc/intel/baytrail/sst-baytrail-ipc.c b/sound/soc/intel/baytrail/sst-baytrail-ipc.c
      --- a/sound/soc/intel/baytrail/sst-baytrail-ipc.c
      +++ b/sound/soc/intel/baytrail/sst-baytrail-ipc.c
      @@ -263,7 +263,7 @@ static struct sst_acpi_desc sst_acpi_baytrail_desc = {
        { "80860F28", (unsigned long)&sst_acpi_baytrail_desc },

    unxz linux-4.1.tar.xz
    tar -tf linux-4.1.tar | grep 'soc/intel/baytrail' # ::
      linux-4.1/sound/soc/intel/baytrail/
      linux-4.1/sound/soc/intel/baytrail/Makefile
      linux-4.1/sound/soc/intel/baytrail/sst-baytrail-dsp.c
      linux-4.1/sound/soc/intel/baytrail/sst-baytrail-ipc.c
      linux-4.1/sound/soc/intel/baytrail/sst-baytrail-ipc.h
      linux-4.1/sound/soc/intel/baytrail/sst-baytrail-pcm.c

    tar -tf linux-4.1.tar | grep 'i915'  # highlights :
      linux-4.1/drivers/gpu/drm/i915/
      linux-4.1/drivers/gpu/drm/i915/Kconfig
      linux-4.1/drivers/gpu/drm/i915/Makefile
      linux-4.1/drivers/gpu/drm/i915/dvo.h
      linux-4.1/drivers/gpu/drm/i915/dvo_ch7017.c
      linux-4.1/drivers/gpu/drm/i915/i915_debugfs.c
      linux-4.1/drivers/gpu/drm/i915/i915_dma.c
      linux-4.1/drivers/gpu/drm/i915/intel_acpi.c
      linux-4.1/drivers/gpu/drm/i915/intel_atomic.c
      linux-4.1/include/drm/i915_component.h
      linux-4.1/include/drm/i915_drm.h
      linux-4.1/include/drm/i915_pciids.h
      linux-4.1/include/uapi/drm/i915_drm.h
      linux-4.1/sound/pci/hda/hda_i915.c

    tar -tf linux-4.1.tar | grep -i 'hdmi'  # highlights (amongst many files) :
      linux-4.1/drivers/gpu/drm/i915/intel_hdmi.c
      linux-4.1/sound/soc/codecs/hdmi.c

    rpm2cpio ../anaconda-23.19-1.1awb.src.rpm | cpio -idmv
    more anaconda-21.25-baytrail.patch  # Net comment is :
      # Baytrail patch: force 800x1280 resolution and bypass shim
      Patch0: anaconda-21.25-baytrail.patch

Intel's Open Source source code site : 
  https://01.org/ubuntu-hdmi
    https://github.com/01org/baytrailaudio
      -- has a baytrail patch in the root folder that contains :
      
      +++ b/drivers/gpu/drm/i915/hdmi_audio_if.h (NEW FILE)
      diff --git a/drivers/gpu/drm/i915/i915_rpm.c b/drivers/gpu/drm/i915/i915_rpm.c ## new file mode 100644    

    In the issues (pkendall64 commented on May 1) : 
      I've built a kernel (3.16.7-ckt9) including your patch and installed the kernel onto a meegopad t01 bay trail-t CR device and rebooted.
      It boots fine and I get a message in the kernel boot logs (dmesg)
      ******* HAD DRIVER loading.. Ver: 0.01.003
      But I do not get any devices listed when I use aplay -l.
      Is there anything I can do to help diagnose the problem?

      I've got the patch working on the meegopad. 
      It seems that the ACPI device HAD0F28 was marked as not ready. 
      Its readiness is controlled by a flag (OSSL) in the DSDT, 
      so I just forced it ready by returning a status of 0x0F and getting grub to provide a custom DSDT. 
      Once I did this the audio came to life.

      Do you know if there's a proper way of controlling the OSSL flag?

Bluetooth comment :
  http://sparkylinux.org/forum/index.php?topic=3269.0
    Solved for external usb dongle using blueman instead of the shipped available bluetooth-mate that does not work, so removed bluetooth-mate completely then installed blueman with synaptic.
    
    mate-bluetooth has been dropped in favor of blueman, but there is no stable release available atm. 
    You should replace it with another bluetooth client, like xcfe4-bluetooth for example.