### Generation of class with get & set accessors
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