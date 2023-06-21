[[C++]]

```c++
#include <iostream>
#include <map>
#include <string>

class Character {
 public:
  virtual ~Character() {}
  virtual void PrintName() = 0;
};

class Warrior : public Character {
 public:
  void PrintName() override { std::cout << "I am a warrior." << std::endl; }
};

class Mage : public Character {
 public:
  void PrintName() override { std::cout << "I am a mage." << std::endl; }
};

typedef Character* (*CreateCharacterFn)();

std::map<std::string, CreateCharacterFn> character_factories;

template <typename T>
Character* CreateCharacter() {
  return new T;
}

template <typename T>
void RegisterCharacter(const std::string& name) {
  character_factories[name] = &CreateCharacter<T>;
}

int main() {
  // Register character types with the engine.
  RegisterCharacter<Warrior>("warrior");
  RegisterCharacter<Mage>("mage");

  // Dynamically create a character.
  std::string type = "warrior";
  Character* character = character_factories[type]();
  character->PrintName();

  delete character;
  return 0;
}
```