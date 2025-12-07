# OpenKSK
Repository for Reproduction PCBs 

## Introduction

This repository contains OSHW Reproduction Parts related to early 1990's Sony KSK Series Laser Boards, which are found in systems such as the Sega CD Model 1 (KSK-1200A), 2 (KSK-1310A) &amp; Neo Geo CD Frontloader (KSK-1230A). See the documentation for details.

These parts are freely available under the terms of the [CERN-OHL-S](https://github.com/starlightk7/OpenKSK/blob/main/LICENSE) license for you to use, study, modify, or sell. The source files are provided as-is with no warranty or entitlement for support to you or any users that you sell manufactured boards to. See [LICENSE](https://github.com/starlightk7/OpenKSK/blob/main/LICENSE) and [NOTICE](https://github.com/starlightk7/OpenKSK/blob/main/NOTICE) for details. Please carefully read these files to understand your obligations if you choose to distribute this project or modifications of it.

**IMPORTANT NOTE FOR PAL USERS:** PAL units usually use a different JVC drive. They are not compatible with the OpenKSK!

## Sample Build

<div><img src="https://starlightk7.github.io/OpenKSK/assets/oksk-intro.jpg" height="380" width="218"/></div>

## Usage

Suggested usage for the documents and boards in this repository:

* Studying the hardware and learning about it's function. It's actually quite interesting to learn how the pieces of of a CD-ROM drive work
* Learn how the pieces of the CD-ROM drive communication with the project's [technical documentation](https://starlightk7.github.io/OpenKSK/#/technical)
* Aiding repairs and failure diagnostics through the created [schematics](https://starlightk7.github.io/OpenKSK/#/schematic) and boardviews
* Repairing broken drives via replacement, reproduction boards and tested replacement [BOMs](https://starlightk7.github.io/OpenKSK/#/bom).
* Building a customized console as a DIY project, for hobby or educational purposes. This board pairs well with [Cosam's Sega CD Reproduction Motherboard](https://github.com/Board-Folk/MegaCD-Main-BD).

## Disclaimer

OpenKSK can be one part of the repair of your unit. However, it is not a cure-all.

**What it does:**
* Allows the clean repair of boards with damaged or missing pads, particularly for the electrolytic capacitors, avoiding bodge wires in a circuit that is quite sensitive.
* Allows a safe re-cap with extremely thick capacitor pads that will not damage easily.
* Gives you an interesting opportunity to learn about how a CD-ROM drive works as you build and bring one online by hand.
* Pairs well with [Cosam's Sega CD Reproduction Motherboard](https://github.com/Board-Folk/MegaCD-Main-BD) for a stylistic look.

**What it does NOT do:**
* Does not fix burned out lasers
* Does not fix dried out rails and gears or other mechanical issues
* Does not fix issues with focus or laser power from improper tuning of the pots
* Does not fix failed chips or other on board components such as the limit switches or motors.
* Does not fix issues from capacitor leaks on the mainboard, which sometimes cause disk read errors instead of the drive.

Please perform diagnostics on your machine before purchasing or ordering a board. Whether you ultimately use OpenKSK as part of your repair or not, we are happy to help you diagnose and repair your original board on the [Discord](https://discord.gg/DjXvPDA6Vv). Please by all means come by and chat.

## Documentation

Please read the [documentation](https://starlightk7.github.io/OpenKSK/) carefully before using the project.

The BOM, schematics, and wide variety of technical information about how this board functions is available in the documentation.

You can also watch an install video from [1upRestorations (English)](https://youtu.be/wNfhOMQv8S0)

## Getting a Board

The project officially distributes boards from [Japan (inexpensive international shipping available)](https://ko-fi.com/starlightk7/shop), the [US](https://1uprestorations.com/collections/console-mod-parts). If you just wish to simply obtain a board for yourself, it will often be cheaper than ordering from the fab directly.

***Support is offered to those who purchased a board from a vendor listed below. No support is provided for fabricating these files on your own. If you choose that route, you should understand what you are doing and are responsible for ensuring it is done right. I am not responsible for any defects that may arise from improperly manufacturing these files, or for defects arising from modifications made by you or your PCB fab during manufacturing. If you produce these boards for sale without becoming an authorized distributor, you are responsible for any and all customer support related to the product as well as your compliance with the project's license***. 

If you have any questions about this, feel free to contact with questions. If you are interested in becoming an authorized distributor for your region, and thus gaining support for your users, please contact me via Discord or Twitter.

The ultimate goal of this project is to eventually document and reproduce all models, but it will take quite some time. In the meantime, I made the best overall parts (in my opinion) first. If you believe in the long term goal, consider subscribing on Ko-Fi; for the $1 tier you'll also gain access to weekly Dev Logs that teach how boards like these are made. Labor aside, donor boards to research, prototypes, and parts are all expensive, and those funds will help offset those costs to complete more boards.

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/starlightk7)

Feel free to come chat with us on Discord: [![demo](https://img.shields.io/discord/1424189475436625921?logo=discord)](https://discord.gg/DjXvPDA6Vv)

All boards in this project are ~35 years old at the time of writing. All relevant patents worldwide have long expired. This project is not affiliated with or endorsed by Sega or Sony.

## Commissions

I do not have time to build commissions on a regular basis, however, members of the community may offer this service in the future. Please contact me if you are interested in providing this service.

## Disclaimer

**WARNING:** Doing a full build is an advanced project. Please carefully read the documentation and required skills and experience before purchasing or ordering a board. Purchasing boards and parts does not guarantee that you can be successful in doing a build. Significant experience in soldering and desoldering, along with a variety of tools are necessary. Your first build will likely not work on its first boot. Patience, analysis, and debugging will likely be necessary to bring the unit online. Order at your own risk. The author provides no guarantee of your success, and also cannot guarantee that any given vendor can fabricate these boards correctly. If you are ordering boards, please consult with your target vendor before ordering if you wish to order a board.

## Parts Index

### Sega CD / Mega CD Model 1 - KSK-1200A

| Part Number  | Description                                    | Region          | Status      | Japan | United States | United Kingdom |
|--------------|------------------------------------------------|-----------------|-------------|-------|---------------|----------------|
| 1-640-117-11 | Used in Japanese Launch Mega CDs               | NTSC-J          | Planned     | N/A   | N/A           | N/A            |
| 1-640-117-12 | Used in very early Japanese Mega CDs           | NTSC-J 　　　　　 | In Progress | N/A   | N/A           | N/A            |
| 1-640-117-13 | Used in most 1991 Japanese Mega CDs            | NTSC-J          | In Progress | N/A   | N/A           | N/A            |
| 1-640-117-14 | Used in US Sega CDs and late Japanese Mega CDs | NTSC-U / NTSC-J | Released    | [Ko-Fi (Direct)](https://ko-fi.com/starlightk7/shop/openksk), [ebay (Direct)](https://www.ebay.com/usr/starlightk7), [etsy (Direct)](https://www.etsy.com/shop/starlightretro/) | [1up Restorations](https://1uprestorations.com/collections/console-mod-parts) | [Retroupgrades (Coming Soon)](https://www.retroupgrades.co.uk/) |

*Note: All models can be upgraded to the released 1-640-117-14, but a 1-wire bypass is needed on F100 for upgrades from 11/12/13.

### Sega CD / Mega CD Model 2 - KSK-1310A

| Part Number  | Description                                     | Region          | Status  | Japan | United States | United Kingdom |
|--------------|-------------------------------------------------|-----------------|---------|-------|---------------|----------------|
| 1-649-453-11 | Used in Sony Drivetrain Sega / Mega CD Model 2s | NTSC-U / NTSC-J | Planned | N/A   | N/A           | N/A            |

### Neo Geo CD Frontloader - KSK-1230A

| Part Number  | Description                                     | Region          | Status  | Japan | United States | United Kingdom |
|--------------|-------------------------------------------------|-----------------|---------|-------|---------------|----------------|
| 1-640-117-21 | Used in the Neo Geo CD Frontloader              | NTSC-J          | Planned | N/A   | N/A           | N/A            |

*Note: This board appears to be electrically identical to 1-640-117-14 with the exception of using different chip/firmware revisions. I have obtained a 1-640-117-21 board to analyze, but have not tested this yet because the host Neo Geo CD does not work for non-disc drive related reasons. I will test after repairing it, but in the meantime if you're adventurous you can try it and report your results. You must use the chips from the original Neo Geo KSK-1230A board.

### Eject Boards

| Part Number  | Description                                                            | Region          | Status  | Japan | United States | United Kingdom |
|--------------|------------------------------------------------------------------------|-----------------|---------|-------|---------------|----------------|
| CMK-81X      | Original version used in the early 1990s                               | NTSC-U / NTSC-J | Released    | [Ko-Fi (Direct)](https://ko-fi.com/starlightk7/shop/openksk), [ebay (Direct)](https://www.ebay.com/usr/starlightk7), [etsy (Direct)](https://www.etsy.com/shop/starlightretro/) | [1up Restorations](https://1uprestorations.com/collections/console-mod-parts) | [Retroupgrades (Coming Soon)](https://www.retroupgrades.co.uk/) |
| CMKS-81X     | Cost-reduction version found in later compatible assemblies            | NTSC-J          | Planned | N/A   | N/A           | N/A            |

### Accessories

| Part                      | Description                                                    | Region      | Status   | Japan | United States | United Kingdom |
|---------------------------|----------------------------------------------------------------|-------------|----------|-------|---------------|----------------|
| Debug & Calibration Board | Helps attach an oscilloscope/logic analyzer to the test points | Region Free | Released | [Ko-Fi (Direct)](https://ko-fi.com/starlightk7/shop/openksk)   | N/A           | N/A |


