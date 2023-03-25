### Generation of class with get & set accessors
```#include <cstdio>
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
