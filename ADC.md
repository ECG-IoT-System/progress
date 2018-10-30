# ADC sample code & doc.
## 簡介 adc_basic.c 流程

![](https://i.imgur.com/tbUkexq.png)

**1.Enable power**

**2.初始化開發版**
```
BOARD_InitPins();
BOARD_BootClockRUN();
BOARD_InitDebugConsole();
```
**3.配置工作模式**
```
ADC_Configuration();
```
Contents:
Initial ADC to default configuration.
Initial ADC Sigma Delta(SD) configuration.
Bandgap voltage.
Calibration VINN value.
Enable ADC.

**4.軟件觸發器**
```
ADC_DosoftwareTrigger();
```

**5.等待訊號轉換完成**

**6.取得轉換結果且印出**
```
adcConvResult = ADC_GetConversionResult(DEMO_ADC_BASE);
fresult = ADC_ConversionResult2Mv(DEMO_ADC_BASE, DEMO_ADC_CHANNEL, DEMO_ADC_CFG_IDX, g_AdcBandgap, g_AdcVinn,adcConvResult);
PRINTF("Original: 0x%x\t Ch: %d\t Result: %f(mv)\r\n", adcConvResult, DEMO_ADC_CHANNEL, fresult);
```

**7.回到第4點重複執行**


## 簡介 adc_burst.c 流程

![](https://i.imgur.com/YbcXayp.png)


**1.Enable power**

**2.初始化開發版**
```
BOARD_InitPins();
BOARD_BootClockRUN();
BOARD_InitDebugConsole();
```
**3.配置工作模式**
```
ADC_Configuration();
```
Contents:
Initial ADC to default configuration.
Initial ADC Sigma Delta(SD) configuration.
Bandgap voltage.
Calibration VINN value.
Enable ADC.

**4.Start burst**
```
ADC_Enable(DEMO_ADC_BASE, true);
```

**5.軟件觸發器**
```
ADC_DosoftwareTrigger();
```

**6.等待訊號轉換完成**

**7.取得轉換結果**
```
data[i] = ADC_GetConversionResult(DEMO_ADC_BASE);
```
若 i < ADC_BURST_EXAMPLE_NUMBER 則會重複執行 6. 7.

**8.Stop burst**
```
ADC_Enable(DEMO_ADC_BASE, false);
```
**9.輸出結果**
```
fresult = ADC_ConversionResult2Mv(DEMO_ADC_BASE, DEMO_ADC_CHANNEL, DEMO_ADC_CFG_IDX, g_AdcBandgap,g_AdcVinn, data[i]);
PRINTF("Original: 0x%x\t Ch: %d\t Result: %f(mv)\r\n", data[i], DEMO_ADC_CHANNEL, fresult);
```
**10.重複執行4.~9.**

## 簡介 adc_dma.c 流程

![](https://i.imgur.com/neJ5ocG.jpg)
 
**1.Enable power**
```
POWER_EnableADC(true);
```
**2.初始化開發板**
```
BOARD_InitPins();
BOARD_BootClockRUN();
BOARD_InitDebugConsole();
```
**3.配置dma**
```
DMA_Init(DMA0);
DMA_EnableChannel(DMA0, DEMO_ADC_DMA_CHANNEL);
DMA_CreateHandle(&g_adcDmaHandle, DMA0, DEMO_ADC_DMA_CHANNEL);
DMA_SetCallback(&g_adcDmaHandle, ADC_ConvertDMACallback, NULL);
DMA_PrepareTransfer(&transferConfig, (void *)&DEMO_ADC_BASE->DATA, &g_AdcConvResult, sizeof(uint32_t), 4, kDMA_PeripheralToMemory, NULL);
DMA_SubmitTransfer(&g_adcDmaHandle, &transferConfig);
```
**4.配置工作模式**
```
ADC_Configuration();
```
Contents:
1. Initial ADC to default configuration.
2. Initial ADC Sigma Delta(SD) configuration.
3. Bandgap voltage.
4. Calibration VINN value.
5. Enable ADC.

**4.啟動Interrupt**
:::info
**以下為無限迴圈(重複執行5~9)**
**5.從鍵盤讀取字元**
```
GETCHAR();
```
**6.開始進行轉換**
```
DMA_StartTransfer(&g_adcDmaHandle);
```
**7.軟件觸發器**
```
ADC_DoSoftwareTrigger(DEMO_ADC_BASE);
```
**8.等待轉換**
```
while (!g_AdcConvSeqCpltFlag)
```
**9.輸出轉換結果**
```
fresult = ADC_ConversionResult2Mv(DEMO_ADC_BASE, DEMO_ADC_CHANNEL, DEMO_ADC_CFG_IDX, g_AdcBandgap, g_AdcVinn, g_AdcConvResult);

PRINTF("Original: 0x%x\t Ch: %d\t Result: %f(mv)\r\n", g_AdcConvResult, DEMO_ADC_CHANNEL, fresult);
```
:::
## 簡介 adc_interrupt.c 流程

 ![](https://i.imgur.com/WESGUee.jpg)

**1.Enable power**
```
POWER_EnableADC(true);
```
**2.初始化開發板**
```
BOARD_InitPins();
BOARD_BootClockRUN();
BOARD_InitDebugConsole();
```
**3.配置工作模式**
```
ADC_Configuration();
```
Contents:
1. Initial ADC to default configuration.
2. Initial ADC Sigma Delta(SD) configuration.
3. Bandgap voltage.
4. Calibration VINN value.
5. Enable ADC.
```
ADC_EnableInterrupts(DEMO_ADC_BASE, (kADC_InterruptMaskEnable | kADC_InterruptDataReadyEnable));
```
:::info
**以下為無限迴圈(重複執行5~8)**
**5.從鍵盤讀取字元**
```
GETCHAR();
```
**6.軟件觸發器**
```
ADC_DoSoftwareTrigger(DEMO_ADC_BASE);
```
**7.等待轉換**
```
while (!g_AdcConvSeqCpltFlag)
```
**8.輸出轉換結果**
```
fresult = ADC_ConversionResult2Mv(DEMO_ADC_BASE, DEMO_ADC_CHANNEL, DEMO_ADC_CFG_IDX, g_AdcBandgap, g_AdcVinn, g_AdcConvResult);
PRINTF("Original: 0x%x\t Ch: %d\t Result: %f(mv)\r\n", g_AdcConvResult, DEMO_ADC_CHANNEL, fresult);
```
:::
