# 測試 Sample Code
## 安裝 MCUXpresso IDE
**1.電腦端 -**
**到 **
https://www.nxp.com/products/wireless/bluetooth-low-energy-ble/a-highly-extensible-platform-for-application-development-of-qn908x:QN9080DK?&tab=In-Depth_Tab&fbclid=IwAR1XlFwtI87WeL7RKqMB6ekQxGZ0ybJe7UYP7OcjTPdl4OVG0wconDAKubA 
**下載 MCUXpresso SDK, MCUXpresso IDE, MCUXpresso Config Tools 三個檔案並解壓縮**

**2.手機端 - 到 google play 下載 IoT Toolbox**
![](https://i.imgur.com/4x0EpVr.png)

## 測試 heart_rate_sensor
1.打開MCUXpresso IDE 並將SDK包匯入
![](https://i.imgur.com/TDE72z2.jpg)

2.匯入sample code
![](https://i.imgur.com/XZsENJe.jpg)

3.勾選 wireless_examples/bluetooth/heart_rate_sensor/freertos

4.勾選 Project Options 裡的UART -> finish

5.點擊 Build
![](https://i.imgur.com/OsPaHra.jpg)

6.點擊 Debug
![](https://i.imgur.com/UdWrFWL.jpg)

7.接上 JP15
![](https://i.imgur.com/S5DV36j.jpg)

8.點擊Step Into 再點擊Resume
![](https://i.imgur.com/VX8xyRu.jpg)

9.按下開發板上的 button1
![](https://i.imgur.com/m7ULVi2.jpg)

10.打開手機的藍芽，開啟 IoT Toolbox，點擊 Heart Rate
![](https://i.imgur.com/8XPzbQF.png)
![](https://i.imgur.com/3uQ0M3G.png)
![](https://i.imgur.com/a2BRUdt.png)

**Finish~**
