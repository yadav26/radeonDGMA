# radeonDGMA
How to enable DGMA (direct graphics memory access) source, library and driver for redhat 7

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
