# Apple T2 audio controller APO for Boot Camp

This package aims to fix dramatically poor built-in speakers output of **2019 & 2020 16-inch MacBook Pro** in Windows 10 & 11 without affecting computer performance.

> [!WARNING]  
> Only **MacBookPro16,1** & **MacBookPro16,4** models are supported. See [this](https://support.apple.com/en-us/HT201608) or [this](https://support.apple.com/en-us/HT201581) support article to identify your model. Do not attempt to use this with any other computer because it may lead to permanent speaker damage of your computer!

The solution in form of a system-wide equalization [APO](https://learn.microsoft.com/en-us/windows-hardware/drivers/audio/windows-audio-processing-objects) (audio processing object) configured with FIR (finite impulse response) convolution files is suggested. This method is latency-free and provides virtually no CPU usage hit since all equalization data is baked-in into two ready-made files (for tweeters and for woofers separately), so no realtime calculations are necessary (as if parametric filters are used). 

**Equalizer APO** was selected as a high quality open source APO implementation to natively apply FIR convolution files exclusively designed for the specified computer models. The measurement data of the specified computer models' speakers output in Asahi Linux against macOS was used as a baseline for FIR convolution files development.

## Installation

1. Launch **Device Manager** and disable the following devices:

    <details>
      <summary>Screenshot for reference</summary>

      ![Device Manager](https://github.com/bananakid/apple-t2-audio-controller-boot-camp/assets/17095595/532967e1-b781-464b-ba29-85808e8624d4)

    </details>

   - Audio inputs and outputs → Speakers (Apple Audio Device)
   - Software components → DolbyAPO Software Device (HSA)
   - Software components → DolbyAPO SWC Device
   - Software devices → Microsoft GS Wavetable Synth
   - Sound, video and game controllers → High Definition Audio Device
     
2. Download and extract the latest package from [releases](https://github.com/bananakid/apple-t2-audio-controller-boot-camp/) (use [7-Zip](https://www.7-zip.org/) or any other unarchiver)

3. Download **Equalizer APO** from [SourceForge](https://sourceforge.net/projects/equalizerapo/files/1.3/EqualizerAPO64-1.3.exe/download) and `Run as administrator`. Specify Destination Folder as `C:\Program Files\Equalizer APO` when prompted (add a space to default `EqualizerAPO` path)
    <details>
      <summary>Screenshot for reference</summary>

      ![Equalizer APO Setup](https://github.com/bananakid/apple-t2-audio-controller-boot-camp/assets/17095595/2f7dda41-873e-4877-bf2f-09ad09fcd3e4)

    </details>

4. Once setup completes, an **Equalizer APO Configurator** is launched automatically and suggests selecting the playback devices. Select `Speakers` `(Apple Audio Device)` and click `Close`
    <details>
      <summary>Screenshot for reference</summary>

      ![Equalize APO Configurator](https://github.com/bananakid/apple-t2-audio-controller-boot-camp/assets/17095595/527a6edb-fcef-4b5e-a091-20191f08a6e6)

    </details>

5. **Equalizer APO Configurator** will suggest restarting the computer to apply changes, **don't** restart yet

6. Go to location where you extracted the latest package from releases to (in step `2`) , select `Install.bat` and `Run as administrator`. This will copy all required APO configuration files to `%ProgramFiles%\Equalizer APO` and apply `Increase Bass` preset.

7. Restart the computer

8. Play some tunes
> [Heat Waves](https://soundcloud.com/glassanimals/heat-waves) by Glass Animals is a good example to see improvement if you don't have some bass-heavy track by hand. Please note it's recommended to test via local file or SoundCloud because YouTube applies Loudness Normalization by default. Before testing anything via YouTube make sure to install [enhanced-h264ify](https://chrome.google.com/webstore/detail/enhanced-h264ify/omkfmpieigblcllmkgbflkikinpkodlk) extension and check `Disable Loudness Normalization` option (otherwise YouTube will do it's normalization and you won't hear high dynamic range this track features). You can compare how poorly Glass Animals track sounds on [YouTube](https://youtu.be/mRD0-GxqHVo) with the default Loudness Normalization applied by first playing it via [SoundCloud](https://soundcloud.com/glassanimals/heat-waves) if you don't want to install [enhanced-h264ify](https://chrome.google.com/webstore/detail/enhanced-h264ify/omkfmpieigblcllmkgbflkikinpkodlk) extension or if your browser doesn't support extensions.

9. If you'll find sound output to be to be too heavy on the lower side (for example, too much bass in certain scenarios may lessen speech output quality if the speech source's quality is mediocre), you can switch to "default" APO configuration on the fly (at any time without restarting the computer):

    <details>
      <summary>Screenshot for reference</summary>

      ![Equalizer APO Editor](https://github.com/bananakid/apple-t2-audio-controller-boot-camp/assets/17095595/a11c23d3-a3fc-44f9-864f-3624ceb45fcb)

    </details>

    - Pause any audio playback
    - Open **Equalizer APO Editor** from Start
    - Turn **off** `Increase Bass` preset (enabled by default)
    - Turn **on** `Default` preset
    - Optionally, close **Equalizer APO Editor**
    - Play any audio

10. If you're happy with the result, launch `Start` → `Windows Administrative Tools` → `Services`, find `Dolby DAX API Service`, stop it and then disable (as you don't need it anymore)

> [!IMPORTANT]  
> While you can enable speaker's system sound enhancements (like `Bass Boost` and `Loudness Equalization`) via **Registy Editor** (i.e. as described by [Naozumi520](https://github.com/Naozumi520/mbp-16-bootcamp-speaker-mod)), I strongly suggest you don't do it as it lessens sound quality system-wide in random ways. A better approach is to enable required effects on per-application basis, i.e. you can set loudness normalization in **VLC**'s or **foobar2000**'s preferences and so on.

## Acknowledgements
This project uses [lemmyg](https://github.com/lemmyg/t2-apple-audio-dsp/tree/speakers_161)'s built-in speakers output measurement data ([macbook_pro_t2_16_1-48k_1.mdat](https://github.com/lemmyg/t2-apple-audio-dsp/tree/speakers_161/docs)) as a baseline as well as his corresponding FIR convolution files that are to match Asahi Linux raw sound output to macOS raw sound output and are produced from the measurement data. This project uses channel mapping model introduced by [Naozumi520](https://github.com/Naozumi520/mbp-16-bootcamp-speaker-mod) in attempt to match built-in speakers output in Boot Camp to macOS. This project was inspired by [chamed](https://github.com/chadmed/asahi-audio)'s project notes and [dechamps](https://github.com/dechamps/apo/)' APO article. **Thank you!**

## Legal Notes

This project is licensed under the terms of the [MIT License](https://opensource.org/license/mit/).

Measurement data used in this project was provided by [lemmyg](https://github.com/lemmyg) under the [MIT License](https://opensource.org/license/mit/).
