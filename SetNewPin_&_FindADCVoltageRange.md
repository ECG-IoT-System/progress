# 設定新腳位 與 找ADC voltage range
## 設定新腳位(假設PIN8 -> PIN9)
#### 1.打開pin_mux.c
(1)define PIN9

![](https://i.imgur.com/Ty5G4vq.png)

(2)在 BOARD_InitPins() 新增 PIN9的資訊

![](https://i.imgur.com/E8pThdY.png)

#### 2.打開fsl_iocon.h
更改port & pin 為9

![](https://i.imgur.com/ft5y3d9.png)

#### 3.打開adc_burst.c
Set DEMO_ADC_CFG_IDX 9

![](https://i.imgur.com/5poCpyH.png)

完成~

## 找 Single-ended ADC voltage range(參考AN12232 QN908x ADC Application Note)
根據 Note 我們知道 ADCx 的 range 有兩組
**Vinn - Vref <= ADCx <= (Vinn + Vref)** , 
**Vss <= ADCx <= Vcc**

先從第一條開始：
比對 fsl_adc.c & adc_burst.c 的 ADC_ConversionResult2Mv()
![](https://i.imgur.com/qoliFsk.png)
![](https://i.imgur.com/x1qUlU8.png)

我們知道 Vref = g_AdcBandgap

從 ADC_Configuration() 得

![](https://i.imgur.com/8VF4uj6.png)

取值約為1.8V
此時 **Vref Gain = 1.5**

再從 fsl_adc.c 的 ADC_GetSdDefaultConfig()

![](https://i.imgur.com/4gVTxEi.png)

得到 Vinn = 0.75 * Vref = 1.35(V)
**-> -0.45V ≦ ADCx ≦ 3.15V**

再看第二條：
Note 內假定 Vss = GND, Vcc = 3.0V
**-> 0V ≦ ADCx ≦ 3.0V**

綜合兩條範圍限制
**0V ≦ ADCx ≦ 3.0V**
(ADC Gain = 1, Vref Gain = 1.5, Vinn = 0.75*Vref, PGA Gain = 1)
