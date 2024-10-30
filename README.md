# ApplockerBypassExternalBinary #

The original project can reflectively load and execute PEs locally and remotely bypassing EDR hooks. This modified version integrates the use of InstallUtil, allowing it to bypass security configurations like AppLocker for executing various binaries, including tools like Ligolo-ng.

This project closely resembles the well-known Invoke-ReflectivePEInjection.ps1, but being built in C# made it easier to implement code specifically designed for antivirus evasion. It includes basic evasion techniques that can be further adapted to bypass more recent antivirus solutions.

----------------------------------------------------------------------------------------

- I haven't found any ressource for bypassing applocker with ligolo-ng, so here it is.

- Huge thanks to @cpu0x00 for https://github.com/cpu0x00/SharpReflectivePEInjection

	

	
