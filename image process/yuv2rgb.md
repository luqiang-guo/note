# YUV2RGB





## 标准协议



### BT.601


```c++
// BT.601 limited range YUV to RGB reference
//  R = (Y - 16) * 1.164             + V * 1.596
//  G = (Y - 16) * 1.164 - U * 0.391 - V * 0.813
//  B = (Y - 16) * 1.164 + U * 2.018
// KR = 0.299; KB = 0.114

// BT.601 full range YUV to RGB reference (aka JPEG)
// *  R = Y               + V * 1.40200
// *  G = Y - U * 0.34414 - V * 0.71414
// *  B = Y + U * 1.77200
// KR = 0.299; KB = 0.114
```

```c++
// R = (298*(Y-16)              +409*(V-128) +128)>>8
// G = (298*(Y-16) -100*(U-128) -208*(V-128) +128)>>8
// B = (298*(Y-16) +516*(U-128)              +128)>>8
```



```c++
constexpr int32_t ITUR_BT_601_CY    = 1220542; // 1.164 * (1 << 20)
constexpr int32_t ITUR_BT_601_CBU   = 2116026; // 2.018 * (1 << 20)
constexpr int32_t ITUR_BT_601_CGU   = -409993; // -0.391 * (1 << 20)
constexpr int32_t ITUR_BT_601_CGV   = -852492; // -0.813 * (1 << 20)
constexpr int32_t ITUR_BT_601_CRV   = 1673527; // 1.596 * (1 << 20)
constexpr int32_t ITUR_BT_601_SHIFT = 20;

//R = (1220542(Y - 16) + 1673527(V - 128)                  + (1 << 19)) >> 20
//G = (1220542(Y - 16) - 852492(V - 128) - 409993(U - 128) + (1 << 19)) >> 20
//B = (1220542(Y - 16)                  + 2116026(U - 128) + (1 << 19)) >> 20
```



```
```




### BT.709

```c++
// BT.709 limited range YUV to RGB reference
//  R = (Y - 16) * 1.164             + V * 1.793
//  G = (Y - 16) * 1.164 - U * 0.213 - V * 0.533
//  B = (Y - 16) * 1.164 + U * 2.112
//  KR = 0.2126, KB = 0.0722

// BT.709 full range YUV to RGB reference
//  R = Y               + V * 1.5748
//  G = Y - U * 0.18732 - V * 0.46812
//  B = Y + U * 1.8556
//  KR = 0.2126, KB = 0.0722
```



## LIBYUV



```c++
  uint32_t y32 = y * 0x0101;  
  int32_t y1 = (uint32_t)(y32 * yg) >> 16;
  int b16 = y1 + (u * ub) - bb;
  int g16 = y1 + bg - (u * ug + v * vg);
  int r16 = y1 + (v * vr) - br
```

