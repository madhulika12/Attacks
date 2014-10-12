Attacks
=======

Injection, DoS and Scanning

MODIFICATIONS 54 - 

-In order to write to the slave PLC, I have to write in the location where the master PLC would write, so I've got the master PLC commented out of the shell script. This will probably change later when I reincorporate the master PLC
-For some reason, for any 1 bit data item I request from the slave PLC through iFix's MBE driver, it subtracts 1 bit from the address I give it. In order to compensate for this, I added 1 to every address I needed for 1 bit data items. The floats(2-bit) seem to work fine. Le weird.

-I modified the PIDSetpoint, PIDGain, and PIDRate values in the mkconfig script to have decimal places. Apparently, iFix struggles if it does have a defined decimal value and at least one non-zero value(thus for 0, I used 0.00001)

-I think the command to open the port logger in the run.sh script actually opens the serial ports. Had to uncomment that one to use the RTU sims.

-I added installing glibc.i686 to the testbedinit file. I think we need it for the attack scripts.

-I edited the config file such that the SP starts up as 5.0 instead of 10.0.

-for Wei's attacks, I'm using some serial-to-MODBUS converter code. Looks like I need to run "./mtu.out localhost". Right now, mtu.c is setup to listen on ttyS2. The attack code needs to be setup for it's output to be ttyS1. These are physical ports right now.

-the serial-to-tcp converter is setup using the mtu.c. The attacks(which are mostly serial) write to serial port 2(which is setup as the client in VMWARE) and the mtu.c converter reads from serial port 3. It then converts this and connects to the IP address 10.128.0.1 port 502 and sends the serial data as TCP packets.

-I have a second VM just for running the attacks at IP 10.128.0.4 that has 2 "physical" serial ports

I did modify the attacks that I'm using, and most of the attacks in the file. I changed to used the serial ports, and I changed the address of the register to be attacked. I suspect this is because I took the master out of the equation. I wrote my own variant of the SP attack that ONLY modifies the setpoint, not all the PID parameters.
