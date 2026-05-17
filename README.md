## EVM56K at a glance
The EVM56K is a DSP56001-based Evaluation Module developed in 1993 at GEDA-ITBA, Buenos Aires, Argentina. Includes 32K x 24 bits of RAM, a monitor program stored in ROM (8K x 24 bits), an RS-232 connection at 19200 bps, and an interface port compatible with the PC/AT bus.

As an evaluation module, its primary intention is to be used as a test environment for pre-compiled programs. For such task, a 56K compiler and linker is required. The evaluation module manual is included in this repository.

<div align="center"><img src="https://github.com/user-attachments/assets/3dd89cca-c24d-468b-8478-cc399a21a7d3" style="width:75%;height:75%;text-align:center;"></img></div>

## Starting operation
The EVM56K requires a single 5V power supply (1A max), and a serial connection at 19200bps, 8 bits, no parity and 1 stop bit. Once connected to the computer, start the terminal emulator and turn on the EVM power supply. The following message should appear on the screen:

<div align="center"><img src="https://github.com/user-attachments/assets/bb5557f9-5d8c-4b31-aa4e-f2aa8bc5e983" style="width:50%;height:50%;text-align:center;"></img></div>

From here, one can enter the following commands:
- R: show processor register status
- M: modify/examine memory
- D: dump memory
- C: alter register value
- L: load a `*.LOD` file from the computer into the EVM
- G: start execution from the address at PC
- T: start execution in trace mode
- B: list/add/remove break points.

For more information on each command, read the EVM manual.

## Sample run
We want to alter the value at $0001 to be $123456. First, we examine the memory to retrieve the previous value. The D command will automatically print out 11 lines of 8 locations each. We can continue dumping the memory to the terminal by pressing `SPACE`, or exit to the prompt by pressing `RETURN`:
```
>D X:0000
X:0000  000000 000000 000000 000000 001020 000100 040000 004100
X:0008  080004 000000 000000 000200 000000 000000 001108 000008
X:0010  000000 000004 000000 020400 002000 800400 011000 020000
X:0018  002000 000000 004000 000000 000000 0000C0 000000 000420
X:0020  000040 200000 002108 000000 000000 040080 008000 000000
X:0028  004000 060000 000000 000000 000000 000000 800000 001000
X:0030  020000 000010 000000 800000 300000 000024 000000 000800
X:0038  800000 000000 000000 000000 000100 000000 000A00 000000
X:0040  800000 000000 000800 000000 004000 000000 000802 000004
X:0048  000000 000000 000000 000008 000200 000000 000000 008000
X:0050  000000 000000 000100 000002 800000 000000 000000 008004

>
```
Then, we alter said location using the M command. Note the M command will skip to the next location automatically after filling in 6 digits. We exit back to the prompt by pressing `RETURN`:
```
>M X:0001
X:0001  000000 123456
X:0002  000000
>
```
We now list the contents of memory again, with the D command. See how location $0001 was altered from $000000 to $123456:
```
>D X:0000
X:0000  000000 123456 000000 000000 001020 000100 040000 004100
X:0008  080004 000000 000000 000200 000000 000000 001108 000008
X:0010  000000 000004 000000 020400 002000 800400 011000 020000
X:0018  002000 000000 004000 000000 000000 0000C0 000000 000420
X:0020  000040 200000 002108 000000 000000 040080 008000 000000
X:0028  004000 060000 000000 000000 000000 000000 800000 001000
X:0030  020000 000010 000000 800000 300000 000024 000000 000800
X:0038  800000 000000 000000 000000 000100 000000 000A00 000000
X:0040  800000 000000 000800 000000 004000 000000 000802 000004
X:0048  000000 000000 000000 000008 000200 000000 000000 008000
X:0050  000000 000000 000100 000002 800000 000000 000000 008004

>
```
Let's alter some memory locations using the M command exclusively. We can skip to the next without altering its contents by pressing `+`, or skip to the previous memory location by pressing `-`. Let's alter memory locations $0010 to $0017 to be $100000, $010000, $001000, ...
```
>M X:0000
X:0000  000000 +
X:0001  123456 +
X:0002  000000 +
X:0003  000000 +
X:0004  001020 +
X:0005  000100 +
X:0006  040000 +
X:0007  004100 +
X:0008  080004 +
X:0009  000000 +
X:000A  000000 +
X:000B  000200 +
X:000C  000000 +
X:000D  000000 +
X:000E  001108 +
X:000F  000008 +
X:0010  000000 100000
X:0011  000004 010000
X:0012  000000 001000
X:0013  020400 000100
X:0014  002000 000010
X:0015  800400 000001
X:0016  011000
>
```
Let's examine back what we just wrote:
```
>D X:0000
X:0000  000000 123456 000000 000000 001020 000100 040000 004100
X:0008  080004 000000 000000 000200 000000 000000 001108 000008
X:0010  100000 010000 001000 000100 000010 000001 011000 020000
X:0018  002000 000000 004000 000000 000000 0000C0 000000 000420
X:0020  000040 200000 002108 000000 000000 040080 008000 000000
X:0028  004000 060000 000000 000000 000000 000000 800000 001000
X:0030  020000 000010 000000 800000 300000 000024 000000 000800
X:0038  800000 000000 000000 000000 000100 000000 000A00 000000
X:0040  800000 000000 000800 000000 004000 000000 000802 000004
X:0048  000000 000000 000000 000008 000200 000000 000000 008000
X:0050  000000 000000 000100 000002 800000 000000 000000 008004

>
```
Now, let's alter the value of the A2 register to be $72. Let's list the current values with R:
```
>R
                X1= 000000     X0= 000000     R7= 0000     N7= 0000     M7= FFFF
                Y1= 000000     Y0= 000000     R6= 0000     N6= 0000     M6= FFFF
     A2= 00     A1= 000000     A0= 000000     R5= 0000     N5= 0000     M5= FFFF
     B2= 00     B1= 000000     B0= 000000     R4= 0000     N4= 0000     M4= FFFF
                                              R3= 0000     N3= 0000     M3= FFFF
     PC= 0000   SR= 0300      CCR= -------    R2= 0000     N2= 0000     M2= FFFF
     LA= 0000   LC= 0000      OMR= 0002       R1= 0000     N1= 0000     M1= FFFF
    SSH= 0000  SSL= 0000       SP= 0000       R0= 0000     N0= 0000     M0= FFFF


>
```
We can alter the value using the C command, which operates similar to M. The monitor skips to the next register automatically, so we exit back to the prompt by pressing the `RETURN` key:
```
>C A2
A2= 00 72
A1= 000000
>
```
Let's read back its value, to see how it changed:
```
>R
                X1= 000000     X0= 000000     R7= 0000     N7= 0000     M7= FFFF
                Y1= 000000     Y0= 000000     R6= 0000     N6= 0000     M6= FFFF
     A2= 72     A1= 000000     A0= 000000     R5= 0000     N5= 0000     M5= FFFF
     B2= 00     B1= 000000     B0= 000000     R4= 0000     N4= 0000     M4= FFFF
                                              R3= 0000     N3= 0000     M3= FFFF
     PC= 0000   SR= 0300      CCR= -------    R2= 0000     N2= 0000     M2= FFFF
     LA= 0000   LC= 0000      OMR= 0002       R1= 0000     N1= 0000     M1= FFFF
    SSH= 0000  SSL= 0000       SP= 0000       R0= 0000     N0= 0000     M0= FFFF


>
```

## On documentation
The included manual is in Spanish. There are no schematics available so far, but some documents on the DSP56001 are available on bitsavers.
