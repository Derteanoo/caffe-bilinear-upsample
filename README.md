这个版本caffe实现了双线性插值。                                                                                                       
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
5.import sys
   1.sys.path
   2.sys.path.pop('/root/caffe/python/')
     sys.path.pop('/opt/caffe/python/')
     sys.pat.append('/workspace/caffe-bilinear-upsample/python/')
6 import caffe
