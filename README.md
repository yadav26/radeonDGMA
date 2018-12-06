# radeonDGMA
*Overview*
Direct Graphic Memory Access (DirectGMA) exposes a part of the GPU memory and makes it accessible to other devices on the bus. By knowing the address of the exposed memory, any device that supports DirectGMA can write directly into GPU memory and vice versa. The GPU can write into the memory of a peer device instead of transferring the data to system memory first. This mechanism can be used to exchange data between AMD FirePro™ GPUs and 3rd party devices or to do peer-to-peer transfers between multiple AMD FirePro GPUs in one system. With today’s PCIE 3.0 technology, DirectGMA is a very efficient way of transferring data at very low latency.

*Key Features*
image001Makes a portion of the GPU memory accessible to other devices
Allows devices on the bus to write directly into this area of GPU memory
Allows GPUs to write directly into the memory of remote devices on the bus supporting DirectGMA
Provides a driver interface to allow 3rd party hardware vendors to support data exchange with an AMD GPU using DirectGMA
Peer-to-Peer Transfers between GPUs
Use high-speed DMA transfers to copy data between the memories of two GPUs on the same system/PCIe bus.
Peer-to-Peer Transfers between GPU and FPGAs
Use high-speed DMA transfers to copy data between the memories of the GPU and the FPGA memory.
DirectGMA for Video
Optimized pipeline for frame-based devices such as frame grabbers, video switchers, HD-SDI capture, and CameraLink devices. See our SDI webpage

*Requirements*
APIs supporting AMD FirePro DirectGMA are : OpenGL®, OpenCLTM, DirectX®
The supported operation systems are: Windows ® 7/8/10 64 Bit and Linux ® 64 Bit
Supported only on selected workstation hardware (AMD FirePro WTM W5x00 and above as well as all AMD FireProTMS series)

*How to enable DGMA (direct graphics memory access) source, library and driver for redhat 7*

DirectGMA_CL
Simple example showing how to use DGMA in OpenCL

System Setup
Your system needs 2 AMD GPUs with the DirectGMA feature, and the latest GPU driver. Plug at least 1 monitor on each GPU.

Enable DirectGMA
Before running your DirectGMA samples, you have to make sure that the DirectGMA feature is enabled:

on Windows: AMD Firepro Control Center -> AMD Firepro -> SDI/DirectGMA -> Check the checkbox of the 2 GPU -> Apply -> Restart your computer.

on Linux: in your terminal, enter the commands :

aticonfig --initial=dual-head --adapter=all -f aticonfig --set-pcs-val=MCIL,DMAOGLExtensionApertureMB,96 aticonfig --set-pcs-u32=KERNEL,InitialPhysicalUswcUsageSize,96 and reboot.

***************************************************************************************************************************************
*******ANOTHER WAY TO ENABLE DGMA IN REDHAT 7 USING AMDGPU-PRO 18.50 DRIVER USING GRUB OPTION*****************************************
 - $ sudo ./amdgpu-pro-install -y --opencl=legacy
  - $ sudo reboot
  - edit grub and add amdgpu.direct_gma_size=64 in kernel command line parameters
  - boot the system (run ctrl+x after adding amdgpu.direct_gma_size grub entry)
  - run below command and observe the output(as attached dmesg.jpg)
    $ dmesg | grep -i gma
- Install AMD-APP-SDK(AMD-APP-SDKInstaller-v3.0.130.136-GA-linux64.tar)
  - sudo sh AMD-APP-SDK-v3.0.130.136-GA-linux64.sh 
  - run clinfo in another terminal and observe devices (you will not see cl_amd_bus_addressable_memory, it is fine)
  - Now you can run any dGMA sample application(Do not check for cl_amd_bus_addressable_memory extension)
- Run dGMA opencl test application(showcases data transfer from E9550<->wx7100)
  - $ ./oclDirectGMA

NOTE- 96MB is the limited size from the vBIOS. To increase DGMA size they need to request new vBIOS with higher dGMA aperture size.
***************************************************************************************************************************************


Prerequisites:
cmake 2.8.12 or higher AMD APP SDK 2.9.1 or higher

Building procedure:
create a build directory cd build cmake Path_to_the_src_directory make (Linux) compile the Project.sln project with Visual Studio (Windows)
