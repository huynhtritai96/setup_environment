#include "LibraryCode.hpp"
#include <gtest/gtest.h>
#include <iostream>
# Unit Test: testing return value of funtion; If return value is
#### Basic type: bool, int, char, std::string,
TEST(TestCountPositives, BasicTest) {
  std::vector<int> inputVector{1, -2, 3, -4, 5, -6, -7};
  int count = countPositives(inputVector);

  ASSERT_EQ(3, count);
}




#### Setup gtest, Unit test
int main(int argc, char **argv) {
  testing::InitGoogleTest(&argc, argv);
  return RUN_ALL_TESTS();
}
