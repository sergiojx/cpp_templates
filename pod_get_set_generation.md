# Generation of class with get & set accessors
## Iteration 1: Fixed size
```
#include <cstdio>
#include <string>
#include <array>
#include <cstdint>
#include <string_view>
#include <tuple>
#include <unordered_map>
#include <typeinfo>
#include <iostream>

using namespace std::literals;

template <typename T1, typename T2, typename T3>
class PodX{

public:

template <int n>
 const auto& get() {
 std::cout << "Get: " << n  << "\n";
 return std::get<n>(inner_tuple_);
 };
 
template <int n, typename Tr>
 void set(Tr val) {
 std::cout << "Set: " << n  << "\n";
 static_assert(std::is_same<std::decay_t<decltype(val)>, std::decay_t<decltype(std::get<n>(inner_tuple_))>>::value, "type of val argument is not equal to the target one \n change the passed type or use literals");
 std::get<n>(inner_tuple_) = val;
};
  std::tuple<T1, T2, T3> inner_tuple_;
};

int main()
{
 
  constexpr auto LEVEL_{0}; 
  constexpr auto LEVEL_NAME_{1}; 
  constexpr auto FACTOR_{2};
  
  PodX<int, std::string, float> cami{};
  
  auto a = cami.get<LEVEL_>();
  auto b = cami.get<FACTOR_>();
  auto c = cami.get<LEVEL_NAME_>();
  
  cami.set<LEVEL_>(123);
  cami.set<LEVEL_NAME_>("zoom"s);
  cami.set<FACTOR_>(3.14f);
 
}
```
## Iteration 2: Variable size with expected size
```
#include <cstdio>
#include <string>
#include <array>
#include <cstdint>
#include <string_view>
#include <tuple>
#include <unordered_map>
#include <typeinfo>
#include <iostream>
#include <type_traits>
using namespace std::literals;

template <class... Types>
class PodX{
public:

template <int n>
 const auto& get() {
 std::cout << "Get: " << n  << "\n";
 return std::get<n>(inner_tuple_to_check_type);
 };
 
 template <int n, typename Tr>
 void set(Tr val) {
 std::cout << "Set: " << n  << "\n";
 static_assert(std::is_same<std::decay_t<decltype(val)>, std::decay_t<decltype(std::get<n>(inner_tuple_to_check_type))>>::value, "type of val argument is not equal to the target one \n change the passed type or use literals");
 
 std::get<n>(inner_tuple_to_check_type) = val;
 (void)val;
};

int fot(int val){
		std::cout << "/-> " << val << "\n";
      return 0;
}

// Ref: https://stackoverflow.com/questions/57246592/getting-total-size-in-bytes-of-c-template-parameter-pack
int expexted_size(){
 int sum = 0;
 using I = int[];
 using P = int[];
 (void)(I{0u, sum += sizeof(Types)...});
(void)(P{0u, fot(sizeof(Types))...});
 return sum;
};

 
private:
// Ref1: C++ Hihg Performance Heterogeneous Containers
// Ref2: https://stackoverflow.com/questions/33511753/how-can-i-generate-a-tuple-of-n-type-ts
std::tuple<Types* ...> inner_pointer_tuple_;
std::tuple<Types ...> inner_tuple_to_check_type;


};

int main()
{
  constexpr auto LEVEL_{0}; 
  constexpr auto SPEED_{1}; 
  constexpr auto PRESSURE_{2}; 
  constexpr auto LEVEL_NAME_{3}; 
 
  
  auto lola = PodX<int, int, float, std::string>();
  std::cout << "Expected size: \n" << lola.expexted_size() << "\n";
  
  auto a = lola.get<LEVEL_>();
  auto b = lola.get<SPEED_>();
  auto c = lola.get<PRESSURE_>();
  auto d = lola.get<LEVEL_NAME_>();
  
  lola.set<LEVEL_>(123);
  lola.set<SPEED_>(12);
  lola.set<PRESSURE_>(3.14f);
  lola.set<LEVEL_NAME_>("zoom"s);
  std::cout << "LEVEL_: " << lola.get<LEVEL_>() << "\n";
  std::cout << "SPEED_: " << lola.get<SPEED_>() << "\n";
  std::cout << "PRESSURE_: " << lola.get<PRESSURE_>() << "\n";
  std::cout << "LEVEL_NAME_: " << lola.get<LEVEL_NAME_>() << "\n";
  auto yoya = PodX<int, int, float, std::array<float, 12>>();
 
}
```
## Iteration 3: Chars as template args
```
#include <stdio.h>
#include <iostream>
#include <vector>
#include <memory>

#include <string>
using namespace std;


#define MAX_CONST_CHAR 100

#define MIN(a,b) (a)<(b)?(a):(b)

#define _(s)\
getChr(s,0),\
getChr(s,1),\
getChr(s,2),\
getChr(s,3),\
getChr(s,4),\
getChr(s,5),\
getChr(s,6),\
getChr(s,7),\
getChr(s,8),\
getChr(s,9),\
getChr(s,10),\
getChr(s,11),\
getChr(s,12),\
getChr(s,13),\
getChr(s,14),\
getChr(s,15),\
getChr(s,16),\
getChr(s,17),\
getChr(s,18),\
getChr(s,19),\
getChr(s,20),\
getChr(s,21),\
getChr(s,22),\
getChr(s,23),\
getChr(s,24),\
getChr(s,25),\
getChr(s,26),\
getChr(s,27),\
getChr(s,28),\
getChr(s,29),\
getChr(s,30),\
getChr(s,31),\
getChr(s,32),\
getChr(s,33),\
getChr(s,34),\
getChr(s,35),\
getChr(s,36),\
getChr(s,37),\
getChr(s,38),\
getChr(s,39),\
getChr(s,40),\
getChr(s,41),\
getChr(s,42),\
getChr(s,43),\
getChr(s,44),\
getChr(s,45),\
getChr(s,46),\
getChr(s,47),\
getChr(s,48),\
getChr(s,49),\
getChr(s,50),\
getChr(s,51),\
getChr(s,52),\
getChr(s,53),\
getChr(s,54),\
getChr(s,55),\
getChr(s,56),\
getChr(s,57),\
getChr(s,58),\
getChr(s,59),\
getChr(s,60),\
getChr(s,61),\
getChr(s,62),\
getChr(s,63),\
getChr(s,64),\
getChr(s,65),\
getChr(s,66),\
getChr(s,67),\
getChr(s,68),\
getChr(s,69),\
getChr(s,70),\
getChr(s,71),\
getChr(s,72),\
getChr(s,73),\
getChr(s,74),\
getChr(s,75),\
getChr(s,76),\
getChr(s,77),\
getChr(s,78),\
getChr(s,79),\
getChr(s,80),\
getChr(s,81),\
getChr(s,82),\
getChr(s,83),\
getChr(s,84),\
getChr(s,85),\
getChr(s,86),\
getChr(s,87),\
getChr(s,88),\
getChr(s,89),\
getChr(s,90),\
getChr(s,91),\
getChr(s,92),\
getChr(s,93),\
getChr(s,94),\
getChr(s,95),\
getChr(s,96),\
getChr(s,97),\
getChr(s,98),\
getChr(s,99),\
getChr(s,100)

#define getChr(name, ii) ((MIN(ii,MAX_CONST_CHAR))<sizeof(name)/sizeof(*name)?name[ii]:0)

template <typename T, char... Chars_>
 class E {

    public:
    string *str;

    E(){
        std::vector<char> vec = {Chars_...};
        str = new string(vec.begin(),vec.end());
    }
     E(T var):var_{var}{
        std::vector<char> vec = {Chars_...};
        str = new string(vec.begin(),vec.end());
    }

    template <char ...Args>
    const T& get();

    template <>
    const T& get<Chars_...>(){
        return var_;
    }

    ~E()
     {
        delete str;
     }

    private:

        T var_;
 };
```

```
#include "tmptlstr.hpp"
#include <array>



int main(int argc, char *argv[])
{

    E<int, _("Any template can pass const strings literals")> e{999};

    E<std::array<float,7>, _("Another Array of floats")> y{{10,200,3000,40000,500000,6000000,70000000}};

    E<std::string, _("greeting")> s{"hEj Hej"};

    printf("%s \n",e.str->c_str());
    printf("%i \n",e.get<_("Any template can pass const strings literals")>());

    printf("%f \n",y.get<_("Another Array of floats")>()[2]);
    printf("%s \n",s.get<_("greeting")>().c_str());
   
}
```
