# 记录常见的快速计算





## exp



**参考链接**

- [ncnn](https://github.com/Tencent/ncnn/blob/master/src/layer/arm/neon_mathfun.h)
- [avx](https://github.com/to-miz/sse_mathfun_extension)
- [caffe2](https://caffe2.ai/doxygen-c/html/avx__mathfun_8h_source.html)
- [ROOT](https://root.cern.ch/doc/v606/exp_8h_source.html)
- [快速计算的近似原理](https://www.zhihu.com/question/51026869/answer/123794632)

## log





## tanh

- [eigen](http://docs.ros.org/en/melodic/api/co_scan/html/MathFunctionsImpl_8h_source.html)

- [连分数](https://math.stackexchange.com/questions/107292/rapid-approximation-of-tanhx)

- 实际计算采用的数值方法

  ![{\frac  {a_{0}}{1}},\qquad {\frac  {a_{1}a_{0}+1}{a_{1}}},\qquad {\frac  {a_{2}(a_{1}a_{0}+1)+a_{0}}{a_{2}a_{1}+1}},\qquad {\frac  {a_{3}[a_{2}(a_{1}a_{0}+1)+a_{0}]+(a_{1}a_{0}+1)}{a_{3}(a_{2}a_{1}+1)+a_{1}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/8eb5374116544ecd01213276fd215d259c86989f)


## 数值判断

- [浮点数对比](https://bot-man-jl.github.io/articles/?post=2020/Comparing-Floating-Point-Numbers)

