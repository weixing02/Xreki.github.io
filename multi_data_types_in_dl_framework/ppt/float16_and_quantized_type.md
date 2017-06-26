
<!-- $size: 16:9 -->

# Float16 and Quantized Int8 Type 

##### Yiqun Liu

---

<!-- page_number: true -->

# Part I：float16 - FP16, half

---

# <small>IEEE 754存储格式</small>

- [float](https://zh.wikipedia.org/wiki/%E5%96%AE%E7%B2%BE%E5%BA%A6%E6%B5%AE%E9%BB%9E%E6%95%B8)
  - 符号位： 1位
  - 指数位： 8位，范围 $2^{-127}$ ~ $2^{126}$
  - 尾数位：23位
    ![50%](images/float32.jpg)
    
- [half](https://zh.wikipedia.org/wiki/%E5%8D%8A%E7%B2%BE%E5%BA%A6%E6%B5%AE%E7%82%B9%E6%95%B0)
  - 符号位： 1位
  - 指数位： 5位，范围 $2^{-15}$ ~ $2^{14}$
  - 尾数位：10位
    ![52%](images/float16.jpg)

---

# <small>类型定义 & 类型转换</small>
- <small>caffe2 
  - <small>类型定义[caffe2/caffe2/core/types.h](https://github.com/caffe2/caffe2/blob/master/caffe2/core/types.h#L53)
    ```cpp
    namespace caffe2 {
    typedef struct CAFFE2_ALIGNED(2) __f16 { uint16_t x; } float16;
    }  // namespace caffe2
    ```
  - 类型转换 [caffe2/caffe2/utils/conversions.h](https://github.com/caffe2/caffe2/blob/master/caffe2/utils/conversions.h#L20)
    ```cpp
    inline float16 cpu_float2half_rn(float f) {
      float16 ret;
      ...
      exponent = ((u >> 23) & 0xff);
      mantissa = (u & 0x7fffff);
      ...
      ret.x = (sign | (exponent << 10) | mantissa);
      return ret;
    }
    inline float cpu_half2float(float16 h) {
      unsigned sign = ((h.x >> 15) & 1);
      unsigned exponent = ((h.x >> 10) & 0x1f);
      unsigned mantissa = ((h.x & 0x3ff) << 13);
	  ...
      unsigned i = ((sign << 31) | (exponent << 23) | mantissa);
      float ret;
      memcpy(&ret, &i, sizeof(i));
      return ret;
    }
    ```
  </small></small>

---

# <small>类型定义 & 类型转换</small>
- Half-precision floating point library [http://half.sourceforge.net/](http://half.sourceforge.net/)

--- 

# FP16为什么比float快？
- 硬件支持-NVIDIA GPU
	- 指令，intrinsic
	- 计算库，cublas，cudnn
- 硬件支持-ARM CPU
	- 指令，intrinsic

---

# 软件实现
- half
- majel
- eigen
- caffe2
- tensorflow

---

# Quantized int8 - Fixed point

---

# Fixed-point
- 原始的方法
- google的方法

---

# INT8计算
- 硬件支持-NVIDIA GPU
	- 指令，intrinsic
	- 计算库，cublas，cudnn
- 硬件支持-ARM CPU
	- 指令，intrinsic
	- 计算库，gemmlowp (google)
- 

---
<!-- prerender: true -->
# Thank You!