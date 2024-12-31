---
sidebar_position: 16
---

# Renesas GPIO Definition
http://140.112.12.82/docu-moil-renesas/assets/files/r01us0488ej0109-rz-g_GPIO_UME-d2505425b42fa90976cf6547fe538323.pdf

RZ/G2L can support up to 123 general-purpose I/O pins from 49 ports ( **Page 8. Table** )

![](../img/gp02_01.png)

### **Formula** :

**Page 14.** in the above document:

![](../img/gp02_02.png)

For example: P42_4

### **Port ID of `P42_4` = 42 * 8 + 4 + 120 = 460**

It is used to export a GPIO pin in userspace.

Table 4-1 Example GPIO device nodes of RZ/G2L ( Page 16. in the above document)

![](../img/gp02_03.png)

### **Test** :

```
cd /sys/class/gpio
echo 460 > export
ls -al
```

A new folder “**P42_4**” will be created.

![](../img/gp02_04.png)

Check the description about P42_4 in the above document,

![GPIO_01-1b00f3c4c34e88ba5b545d11e560959e.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/41f30fda-f209-4ad6-af17-1d205944b6dd/78c9296b-51ce-4fd8-ad61-f3f012a6f771/GPIO_01-1b00f3c4c34e88ba5b545d11e560959e.jpg)
![](../img/gp02_05.jpg)

Continue the below testing:

```
echo 1 > value ( error )
cat direction
in
echo “out” > direction
cat direction
out
echo 1 > value
cat value
echo 0 > value
cat value
```

![](../img/gp02_06.png)
![](../img/gp02_07.png)
![](../img/gp02_08.png)

### **Carrier Board**

Now we want to **find out the pin** corresponding. Check the document of the carrier board.

Check the item

```
"Documentation"/ "Manual - Development Tools"/
"RZ SMARC Series Carrier Board User's Manual:Hardware"
```

In the below Renesas document,

[RZ/G2L-EVKIT - Evaluation Board Kit for RZ/G2L MPU](https://www.renesas.com/en/products/microcontrollers-microprocessors/rz-mpus/rzg2l-evkit-evaluation-board-kit-rzg2l-mpu#documents)

![](../img/gp02_09.png)
![](../img/gp02_10.png)
![](../img/gp02_11.png)
![](../img/gp02_12.png)
![](../img/gp02_13.png)

Find out where is the hardware pin for P42_4?

[RZ SMARC Series Carrier Board User’s Manual: Hardware](http://140.112.12.82/docu-moil-renesas/assets/files/r01uh0966ej0122-rz-RTK97X4XXXB00000BE-9bfd716ef96e14f68272c6a65f662578.pdf)

Check Page 34, Figure 2.16 Block Diagram of PMOD1 I/F

![](../img/gp02_14.png)

### The third one in the bottom row:

![](../img/gp02_15.png)