想起前端时间关于USB2.0在老的嵌入式设备上role切换的问题。不做USB OTG ID PIN的设备树配置，应该是不能在用户空间直接实现主从模式切换。  
USB2.0一般是有这么四个基本引脚：  

VBUS,即USB电源引脚，提供5V电源给USB设备；  
D+, D- 正负数据线，用于传输差分数据信号；  
GND 接地引脚。  

要实现OTG的功能是需要额外一个ID PIN(引脚)，需要在设备树里配置的。 


下面的过程是从网上搜索到。  

USB OTG需要设备能够自动检测USB线的插入方向,并根据插入方向切换为USB主设备或USB从设备。这个判断通常需要使用OTG ID引脚。在设备树中,需要:  
1. 根据硬件设计,找到OTG ID引脚所对应的GPIO。这通常在数据手册中能找到。  
2. 将该GPIO配置为OTG ID功能:
```bash
dts
gpio@XXXX {
    otg-id-pin: otg-id-pin@0 {
        gpio-controller;
        #gpio-cells = <2>;
        compatible = "gpio-otg-id";         
    };
};
```
3. 在USB控制器节点,使用`otg-parent`和`dr_mode`属性关联OTG ID引脚和设置OTG模式:
```bash
dts 
usb@XXXX {
    otg-parent = <&otg-id-pin>;
    dr_mode = "otg";
    ...
}
```
4. 重新编译设备树并烧写。配置完成后,USB控制器会自动检测OTG ID引脚的状态,并设置为USB主设备(A装置)或USB从设备(B装置)模式。  
这样就实现了USB OTG自动切换功能。具体的设备树配置还需要根据您使用的硬件设计及控制器来定制。但OTG ID引脚的配置思路是相同的。
