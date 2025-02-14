## Matrix-matrix and matrix-vector multiplication  
* **operators**:  
> binary operators: *  
> compound operators: *=  
  

   
```cpp
#include <iostream>
#include <Eigen/Dense>

int main()
{
    Eigen::Matrix2d mat;
    mat << 1, 2,
           3, 4;
    Eigen::Vector2d u(-1,1), v(2,0);
    std::cout << "Here is mat*mat:\n" << mat*mat << std::endl;
    std::cout << "Here is mat*u:\n" << mat*u << std::endl;
    std::cout << "Here is u^T*mat:\n" << u.transpose()*mat << std::endl;
    std::cout << "Here is u^T*v:\n" << u.transpose()*v << std::endl;
    std::cout << "Here is u*v^T:\n" << u*v.transpose() << std::endl;
    std::cout << "Let's multiply mat by itself" << std::endl;
    mat = mat*mat;
    std::cout << "Now mat is mat:\n" << mat << std::endl;
}  
```    
  
--------------------------------------------   
## Dot product and cross product    
* **methods**:  
> dot()  
> cross()  
  
```cpp  

#include <iostream>
#include <Eigen/Dense>
 
int main()
{
  Eigen::Vector3d v(1,2,3);
  Eigen::Vector3d w(0,1,2);
 
  std::cout << "Dot product: " << v.dot(w) << std::endl;
  double dp = v.adjoint()*w; // automatic conversion of the inner product to a scalar
  std::cout << "Dot product via a matrix product: " << dp << std::endl;
  std::cout << "Cross product:\n" << v.cross(w) << std::endl;
}  
```  
  
!!! note  

    <center>Remember that cross product is only for vectors of size 3. Dot product is for vectors of any sizes. When using complex numbers, Eigen's dot product is conjugate-linear in the first variable and linear in the second variable.</center>  
      
        
--------------------------------------------  

## Basic arithmetic reduction operations  
* **methods**:  
> sum()  
> mean()    
> prod()  
> minCoeff()  
> maxCoeff()  
> trace()  


