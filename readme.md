**About**
======
  
The Q9 developer site(https://q9.developer.lge.com) offers tool-chains, sample apps, open APIs, an API simulator, and other related documentation. Developers can use these tools and resources to build their own robot services for Q9.
Even without prior knowledge of Yocto, they can easily and quickly set up a development environment for building new Q9 applications. By following the tutorials that we provided, developers can write their new codes to create installation files. These installation files can be directly deployed into Q9 through the site.
Developers can also create the apps using the combination of open APIs. We will continuously update core API functions in various categories.
The API simulator is a visual programming editor based on Google Blockly, offering a drag-and-drop interface for API blocks.
Publish and share your applications via the  APP market page. If you encounter issues or have questions, you can use the community page, which offers FAQs and Q&A features.

<br/> 

**Overview**
======

**Background**
------
  

LG Electronics unveiled the 'Smart Home AI Agent' at CES 2024 to accelerate the realization of 'Zero Labor Home, Makes Quality Time'.  
The AI agent can move around the house on two wheels and conveniently connects and controls home appliances and IoT devices. It can acquire various information based on voice, sound, and image information obtained through AI functions, and this is used to help people with their housework. The AI agent is designed as an open platform based on ROS 2, and has been developed to allow developers to add additional software functions implemented by developers in addition to the functions provided by the agent through a standardized interface.LG Electronics plans to expand the open ecosystem that provides opportunities for developers to directly participate in the ecosystem and enable co-development to develop products with customers.

  


<br/> 
<br/> 
<br/> 
<br/>   

  



-----------

  

**Why ROS 2?**
-----
  

ROS is an abbreviation for Robot Operating System, and is an open source-based robot meta-operating system that appeared in 2007. It provides a software framework for data transmission and reception, scheduling, and error handling between heterogeneous hardware. It also provides various drivers, libraries, and development tools as open packages to build an ecosystem in the robotics field. According to ABI Research, it is predicted that 55% of all commercial robots will apply ROS in 2024, and recently, robot manufacturers, cloud, OS, and chipset companies such as iRobot, Toyota, Sony, Amazon, Microsoft, Intel, and Qualcomm have been continuously expanding their application and support of ROS2. ROS 2 was released in December 2017, and it reflects the overall equirements of the industry while accepting the advantages of ROS as it is and supplementing its shortcomings. It has resolved the gap between prototype and production, and adopted DDS (Data Distribution Service), a data-centric communication standard for distributed environments, to support real-time and embedded systems as  middleware communication framework. It is capable of controlling multi-robot systems and can be integrated with small platforms such as microcontrollers without an OS installed.

  

  <br/> 
  <br/> 
  <br/> 
  <br/> 

  

  

**Why Yocto Project?**
-----
  

The Yocto Project is an open source collaborative project that provides templates, tools, and methods for developers to develop embedded Linux-based systems regardless of hardware architecture. Its goal is to create a customizable Linux distribution that can run on any embedded hardware. It is a collection of several open sources and provides an integrated build system for hardware-independent embedded product design. It currently supports seven processors: ARM, ARM64, x86, x86-64, PowerPC, MIPS, and MIPS64, and provides a build environment, utilities, and development tools to reduce the dependency of the working environment between developers.  
Linux refers to the OS kernel, not each individual part that makes up the entire system, and a Linux istribution consists of a bootloader, Linux kernel, and root file system. Yocto is a tool that can create a Linux distribution, and does not mean the Linux distribution itself. On the other hand, Ubuntu is a Linux distribution based on Debian and consists of open source software, and it is not a tool for creating a Linux distribution. 
In other words, Yocto and Ubuntu have different personalities, so they each have their own advantages and isadvantages in developing and maintaining embedded-based Linux systems.
Ubuntu has the advantage of rapid prototyping and Proof of Concept because it configures the system using a Linux distribution. However, it requires hardware that supports Ubuntu and requires a lot of memory. It also has the disadvantage of hardware portability and difficulty in maintenance through history management. On the other hand, Yocto is suitable for customized embedded environments that support various hardware, and has advantages in hardware portability, maintenance, and history management. The advantages and disadvantages of Ubuntu and Yocto are described in detail in the table below. In conclusion, Ubuntu has the advantage of rapid prototyping and Proof of Concept. However, considering the support for robots of various form factors, hardware portability, boot time, memory, license management, and maintenance aspects, a lightweight OS is required through a bottom-up customized design, and robot development based on ROS2 on Yocto is required to maximize performance and optimize robot resources through linkage with the cloud.  

<br/> 
<br/>

  

|     |     |     |
| --- | --- | --- |
| Issue | Yocto | Ubuntu |
| Compile | Fully cross compile | Target board or cross compile |
| Configuration, Customization | The Yocto Project is an open-source collaboration project that provides templates, tools, <br><br>and methods for developers to develop embedded Linux-based systems regardless of hardware architecture,<br><br>Hardware portability, easy maintenance<br><br>When configuring the same platform based on x86, arm64,<br><br>Bootloader, Kernel Only Changeable | Hardware portability, difficult to maintain<br><br>  <br><br>When configuring the same platform based on x86, arm64,<br><br>  <br><br>Need to develop separately |
| Reproducibility | Build fully automated build systems | Manually select a package to create an image<br><br>Create with a script tool created by a developer |
| Patching | Need a patch to match the target chipset | Unable to predict and debug the impact of Patch on the system |
| License | When you create a distribution, it is included automatically. | Requires manual extraction |
| Boot time | Optimized Boot Time with Bottom-up Development Method | Increased boot time with unnecessary service inclusion |
| Memory | Minimize and optimize the package configuration you need | Larger with unnecessary packages and patches |
| Learning curve | Developers avoid learning because it's hard to learn | Develops like writing a Linux desktop |

  <br/> 
  <br/> 
  <br/> 
  <br/> 
 

**SW Architecture**
-----

![Image](https://github.com/user-attachments/assets/48ccd656-30bf-4465-a5e1-93c4c763dcb5)  

The software architecture of ROS2 on the Yocto-based Q9 consists of a total of eight layers:  
Yocto Project (including Open Embedded), Chipset (SoC), ROS 2, 3rd Party, Robot Engines, 3rd Party Library, Browser, Robot Manager and SDK. The Yocto Project layer is provided by the Yocto Project, while the 3rd Party Library layer can include various open packages such as Navigation2 and SLAM Toolbox.
The Chipset layer consists of layers provided by chipset manufacturers, such as meta-qcom, meta-rockchips, and meta-tegra. In the Q9, Qualcomm's RB5 (QRB5165) is applied,and device drivers have been implemented to operate the components of the robots. The Meta-ros layer is designed for the integration of ROS 2, and considering the embedded environment (CPU, memory), LG Electronics has applied a mmap-based shared memory developed in-house to optimize the processing of large data, such as camera images.

The Robot Engines Layer is for the integration of navigation, interaction, and device engines (a collection of ROS 2 nodes) in the Q9. The Browser layer supports HTML5-based WebApps, and in the Q9, Chromium has been integrated. The Robot Manager consists of the Robot Launcher Manager, which automatically executes chipset/network-related settings, logging configurations, and ROS 2 nodes upon robot booting, and the Robot State Manager, which manages the states of ROS2 nodes according to the robot's status. 
The Robot Engines Layer is for the integration of navigation, interaction, and device engines (a collection of ROS2 nodes) in the Q9. Finally, the SDK supports external developers in implementing desired robot services on the Q9 using the Toolchain, open API, and Sample App. 
The layers are designed to separate common parts (Yocto, ROS2) and variable parts (chipset layer), enabling rapid software version updates for the common parts. Through the combination of common and variable parts, it is easy to expand software versions that support various form factors for different products and chipsets of the robots.
 
 
  <br/> 
  <br/> 
  <br/> 
  <br/> 


**Q9 Hardware Specifications and Software Versions**
----------------------------------------------------

  

Q9 Hardware Specifications and Software Versions  

<br/> 
<br/>   

  
| Hardware Specifications | Specification |
| --- | --- |
| CPU | 1) CPU  0, 1, 2, 3 (CLUSTER_LITTLE) : Kryo 585 Silver,  2.1 GHz<br><br>2) CPU 4, 5, 6(CLUSTER_BIG) : Kryo 585 Gold ,  2.42 GHz<br><br>3) CPU 7(CLUSTER_PRIME) : Prime Kryo 585 Gold+ core operating at 2.84 GHz |
| Flash Memory | 128 GB UFS 3.1 |
| AP  | Qualcomm QRB5165 (Robotics RB5) |
| RAM | RAM, 8GB, LPDDR5 (POP) |


 <br/> 
<br/> 

  
| Key Software Versions | Software version |
| --- | --- |
| ROS 2 | Humble Hawksbill |
| Yocto | Code name : Dunfell (version 3.1) |
| Kernel | Linux Kernel 4.18.157 |
| Chromium | 101.0.4951.54 |
| Navigation 2 | 1.1.7 |
| slam-toolbox | 2.6.0-2 |
| OpenCV | 4.1.0 |
