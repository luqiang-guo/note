# NCNN





## 简单上手使用

以yolov5 为例子

- **1**  实例化  ncnn::Net yolov5

- **2**  加载param

  ```c++
      yolov5.load_param("yolov5s_6.0.param");
      yolov5.load_model("yolov5s_6.0.bin");
  ```

  

- **3**  预处理

  ```c++
  ncnn::Mat::from_pixels_resize(...);
  ncnn::copy_make_border(...);
  in_pad.substract_mean_normalize(...);
  
  ```

  

- **4**   创建Extractor

  ```c++
  ncnn::Extractor ex = yolov5.create_extractor();
  ```

  

- **5**  设置输入

  ```c++
  ex.input("images", in_pad);
  ```

  

- **6**  推理 extract

  ````
  ex.extract("output", out);
  ex.extract("401", out);
  ````

  

- **7** 后处理

  

 ##  NCNN 数据结构



###  Mat  

类似torch的  tensor   CV的 Mat。



## NCNN 类



### Net

#### custom layer

```c++
   // register custom layer by layer type name
    // return 0 if success
    int register_custom_layer(const char* type, layer_creator_func creator, layer_destroyer_func destroyer = 0, void* userdata = 0);
    virtual int custom_layer_to_index(const char* type);
    // register custom layer by layer type
    // return 0 if success
    int register_custom_layer(int index, layer_creator_func creator, layer_destroyer_func destroyer = 0, void* userdata = 0);
```

#### Load***

```c++
#if NCNN_STRING
    int load_param(const DataReader& dr);
#endif // NCNN_STRING

    int load_param_bin(const DataReader& dr);

    int load_model(const DataReader& dr);

#if NCNN_STDIO
#if NCNN_STRING
    // load network structure from plain param file
    // return 0 if success
    int load_param(FILE* fp);
    int load_param(const char* protopath);
    int load_param_mem(const char* mem);
#endif // NCNN_STRING
    // load network structure from binary param file
    // return 0 if success
    int load_param_bin(FILE* fp);
    int load_param_bin(const char* protopath);

    // load network weight data from model file
    // return 0 if success
    int load_model(FILE* fp);
    int load_model(const char* modelpath);
#endif // NCNN_STDIO

    // load network structure from external memory
    // memory pointer must be 32-bit aligned
    // return bytes consumed
    int load_param(const unsigned char* mem);

    // reference network weight data from external memory
    // weight data is not copied but referenced
    // so external memory should be retained when used
    // memory pointer must be 32-bit aligned
    // return bytes consumed
    int load_model(const unsigned char* mem);
```

#### Bolb  Layer

```c++
    const std::vector<Blob>& blobs() const;
    const std::vector<Layer*>& layers() const;

    std::vector<Blob>& mutable_blobs();
    std::vector<Layer*>& mutable_layers();

    int find_blob_index_by_name(const char* name) const;
    int find_layer_index_by_name(const char* name) const;
```



#### Extractor;

```
friend class Extractor;
```

#### NetPrivate

```
NetPrivate* const d;
```

### NetPrivate

#### forward_layer

```c++
int forward_layer(int layer_index, std::vector<Mat>& blob_mats, const Option& opt) const;
int do_forward_layer(const Layer* layer, std::vector<Mat>& blob_mats, const Option& opt) const;
```

#### Blob Layer

```c++
std::vector<Blob> blobs;
std::vector<Layer*> layers;

std::vector<int> input_blob_indexes;
std::vector<int> output_blob_indexes;

std::vector<const char*> input_blob_names;
std::vector<const char*> output_blob_names;
```

#### Pool

```c++
PoolAllocator* local_blob_allocator;
PoolAllocator* local_workspace_allocator;
```

### Layer

####  Load

```c++
    // load layer specific parameter from parsed dict
    // return 0 if success
    virtual int load_param(const ParamDict& pd);

    // load layer specific weight data from model binary
    // return 0 if success
    virtual int load_model(const ModelBin& mb);
```

####  data 

```c++
public:
    // custom user data
    void* userdata;
    // layer type index
    int typeindex;
#if NCNN_STRING
    // layer type name
    std::string type;
    // layer name
    std::string name;
#endif // NCNN_STRING
    // blob index which this layer needs as input
    std::vector<int> bottoms;
    // blob index which this layer produces as output
    std::vector<int> tops;
    // shape hint
    std::vector<Mat> bottom_shapes;
    std::vector<Mat> top_shapes;
```

### Blob

#### blob name

```c++
 std::string name;
```

#### Producer

```c++
  // layer index which produce this blob as output

  int producer;
```

#### consumer

```c++
  // layer index which need this blob as input
  int consumer;
```

#### Shape

```c++
  // shape hint
  Mat shape;
```



## NCNN OP 注册方式



所有的op 都继承自Layer



### 注册方式

CMakeLists.txt 中注册

```
# layer implementation
ncnn_add_layer(AbsVal)
ncnn_add_layer(ArgMax OFF)
ncnn_add_layer(BatchNorm)
```



```c++
#define DEFINE_LAYER_CREATOR(name)                          \
    ::ncnn::Layer* name##_layer_creator(void* /*userdata*/) \
    {                                                       \
        return new name;                                    \
    }

#define DEFINE_LAYER_DESTROYER(name)                                      \
    void name##_layer_destroyer(::ncnn::Layer* layer, void* /*userdata*/) \
    {                                                                     \
        delete layer;                                                     \
    }
    
```

