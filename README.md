很多工程项目中部署模型都需要采样caffe,但是原版caffe并不支持双线性插值操作。双线性插值在目标检测和图像分割模型中被经常使用，原版中不支持上采样，这为工程模型的工程部署造成很大麻烦，该版本caffe参照博客，加入插值操作，可实现上采样，但与双线性插值还有点不同。以下是使用方法：
                                                                                                       
1 我建议使用官方提供的caffe镜像         

2 运行caffe镜像生成容器

3 cd  /workspace                                                                                                          

4 git clone ........                                                                                                             

5 cd caffe-bilinear-upsample  

3.在容器中编译该版本caffe                                                                                                        
    1)make clean                                                                                                              
    2)make -j8                                                                                                                 
    3)make pycaffe
    
4.打开python交互式环境  

5.import  sys                                                                                                                 
   1)sys.path                                                                                                                 
   2)sys.path.pop('/root/caffe/python/')                                                                                      
   3)sys.path.pop('/opt/caffe/python/')                                                                                       
   4)sys.path.append('/workspace/caffe-bilinear-upsample/python/')   
   
6 import caffe                                                                                                                



