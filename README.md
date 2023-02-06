#include <iostream>
#include <SFML/Graphics.hpp>
#include <vector>

const int WIDTH = 1200;
const int HEIGHT = 800;

struct Level {
    sf::Texture backgroundTexture;
    sf::Sprite backgroundSprite;
    std::vector<sf::Vector2f> platforms;
    std::vector<Coin> coins;
    std::vector<Enemy> enemies;
    std::vector<Weapon> weapons;
};

class Coin {
 public:
  Coin(const sf::Vector2f& position)
      : sprite_(), position_(position), collected_(false) {
    if (!texture_.loadFromFile("coin.png")) {
      std::cout << "Error loading coin texture" << std::endl;
    }
    sprite_.setTexture(texture_);
    sprite_.setPosition(position_);
  }

  void Draw(sf::RenderWindow& window) {
    if (!collected_) {
      window.draw(sprite_);
    }
  }

  void Collect() { collected_ = true; }

  bool IsCollected() const { return collected_; }

  sf::FloatRect GetBounds() const { return sprite_.getGlobalBounds(); }

 private:
  sf::Texture texture_;
  sf::Sprite sprite_;
  sf::Vector2f position_;
  bool collected_;
};

class Enemy {
 public:
  Enemy(const sf::Vector2f& position)
      : sprite_(), position_(position), speed_(2.f) {
    if (!texture_.loadFromFile("enemy.png")) {
      std::cout << "Error loading enemy texture" << std::endl;
    }
    sprite_.setTexture(texture_);
    sprite_.setPosition(position_);
  }

  void Update(const float delta_time) {
    position_.x -= speed_ * delta_time;
    sprite_.setPosition(position_);
  }

  void Draw(sf::RenderWindow& window) { window.draw(sprite_); }

 private:
  sf::Texture texture_;
  sf::Sprite sprite_;
  sf::Vector2f position_;
  float speed_;
};

class Weapon {
 public:
  sf::Vector2f position;
  sf::Texture texture;
  sf::Sprite sprite;
  
  Weapon(const sf::Vector2f &pos, const std::string &texture_file) {
    position = pos;
    if (!texture.loadFromFile(texture_file)) {
      std::cout << "Error loading weapon texture" << std::endl;
    }
    sprite.setTexture(texture);
    sprite.setPosition(position);
  }
};

class Health {
 public:
  int currentHealth;
  int maxHealth;
  
  Health(int max) : currentHealth(max), maxHealth(max) {}
};

class Character {
 public:
  // ...
  Health health;

  Character(const sf::Vector2f &pos, const std::string &texture_file, int maxHealth)
      : position(pos), health(maxHealth) {
    // ...
  }

  // ...
};

class Settings {
 public:
  bool fullscreen;
  int volume;
  
  Settings() : fullscreen(false), volume(50) {}

    class Item {
     public:
      std::string name;
      int price;
      
      Item(const std::string &name, int price) : name(name), price(price) {}
    };

    class Store {
     public:
      std::vector<Item> items;
      int playerCoins;
      
      Store() : playerCoins(0) {}

      void DisplayMenu(sf::RenderWindow &window) {
        // Display the store menu
        // ...
      }

      void HandleEvent(const sf::Event &event) {
        // Handle events related to the store menu
        // ...
      }
    };
    
    
void DisplayMenu(sf::RenderWindow &window) {
    // Display the settings menu
    // ...
  }

void HandleEvent(const sf::Event &event) {
    // Handle events related to the settings menu
    // ...
  }
};

void UpdateHealth(Character &character, int damage) {
  character.health.currentHealth -= damage;
  if (character.health.currentHealth < 0) {
    character.health.currentHealth = 0;
  }
}

void DisplayHealth(sf::RenderWindow &window, Character &character) {
  sf::RectangleShape healthBar(sf::Vector2f(100, 20));
  healthBar.setFillColor(sf::Color::Red);
  healthBar.setPosition(character.position.x, character.position.y - 30);
  window.draw(healthBar);

  sf::RectangleShape healthBarFill(
      sf::Vector2f((float)character.health.currentHealth / character.health.maxHealth * 100, 20));
  healthBarFill.setFillColor(sf::Color::Green);
  healthBarFill.setPosition(character.position.x, character.position.y - 30);
  window.draw(healthBarFill);
}

void StartScreen(sf::RenderWindow &window) {
  sf::Font font;
  if (!font.loadFromFile("font.ttf")) {
    std::cout << "Error loading font" << std::endl;
  }
  
  sf::Text title;
  title.setFont(font);
  title.setString("Super Mario Clone");
  title.setCharacterSize(36);
  title.setFillColor(sf::Color::White);
  title.setPosition((WINDOW_WIDTH - title.getLocalBounds().width) / 2,
                    (WINDOW_HEIGHT - title.getLocalBounds().height) / 2);
  
  sf::Text pressStart;
  pressStart.setFont(font);
  pressStart.setString("Press any key to start");
  pressStart.setCharacterSize(24);
  pressStart.setFillColor(sf::Color::White);
  pressStart.setPosition((WINDOW_WIDTH - pressStart.getLocalBounds().width) / 2,
                         (WINDOW_HEIGHT - pressStart.getLocalBounds().height) / 2 + 50);
  
  while (true) {
    window.clear(sf::Color::Black);
    window.draw(title);
    window.draw(pressStart);
    window.display();
    
    sf::Event event;
    while (window.pollEvent(event)) {
      if (event.type == sf::Event::Closed) {
        window.close();
        exit(0);
      }
      if (event.type == sf::Event::KeyPressed) {
        return;
      }
    }
  }
}

int main()
{
    // Create the window
    sf::RenderWindow window(sf::VideoMode(WIDTH, HEIGHT), "Moe and Gs 2D Game");
    
    // Load textures
    sf::Texture playerTexture;
    playerTexture.loadFromFile("player.png");
    
    sf::Texture backgroundTexture;
    backgroundTexture.loadFromFile("background.png");
    
    std::vector<Level> levels;
    for (int i = 0; i < 8; i++) {
      Level level;
      if (!level.backgroundTexture.loadFromFile("background" + std::to_string(i) + ".png")) {
        std::cout << "Error loading background texture" << std::endl;
      }
      level.backgroundSprite.setTexture(level.backgroundTexture);
        
        Settings settings;

        while (window.isOpen()) {
          sf::Event event;
          while (window.pollEvent(event)) {
            if (event.type == sf::Event::Closed) {
              window.close();
            }

            // Handle events related to the settings menu
            if (event.type == sf::Event::KeyPressed && event.key.code == sf::Keyboard::Escape) {
              settings.DisplayMenu(window);
            }

            settings.HandleEvent(event);
          }

          // ...
        }

        return 0;
      }
    
    while (window.isOpen()) {
      sf::Event event;
      while (window.pollEvent(event)) {
        if (event.type == sf::Event::Closed) {
          window.close();
        }

        // Handle events related to the store menu
        if (event.type == sf::Event::KeyPressed && event.key.code == sf::Keyboard::Tab) {
          store.DisplayMenu(window);
        }

        store.HandleEvent(event);
      }

      // ...
    }

    return 0;
  }
  Note that this is just a basic example, and you may need to modify it to fit the specific requirements of your game.
    
    // Create Sprites
    //New Player
    sf::Sprite playerSprite;
    //Texture package for character
    playerSprite.setTexture(playerTexture);
    //Size of the character
    playerSprite.setPosition(WIDTH / 3, HEIGHT / 6);
    
    // Load the levels
    std::vector<Level> levels;
    // Load level 1
    Level level1;
    level1.platforms = {
        {0.f, 500.f},
        {100.f, 400.f},
        {200.f, 300.f},
    };
    level1.coins = {
        {{50.f, 450.f}},
        {{150.f, 350.f}},
        {{250.f, 250.f}},
    };
    level1.enemies = {
        {{300.f, 500.f}},
        {{800.f, 400.f}},
    };
    level1.weapons = {
        {{500.f, 500.f}, "weapon1.png"},
        {{600.f, 400.f}, "weapon2.png"},
    };
    levels.push_back(level1);
    // Load level 2
    Level level2;
    level2.platforms = {
        {0.f, 500.f},
        {100.f, 400.f},
        {200.f, 300.f},
    };
    level2.coins = {
        {{50.f, 450.f}},
        {{150.f, 350.f}},
        {{250.f, 250.f}},
    };
    level2.enemies = {
        {{300.f, 500.f}},
        {{400.f, 400.f}},
    };
    level2.weapons = {
        {{500.f, 500.f}, "weapon1.png"},
        {{600.f, 400.f}, "weapon2.png"},
    };
    levels.push_back(level2);
    //Load level 3
    Level level3;
    level3.platforms = {
        {0.f, 500.f},
        {100.f, 400.f},
        {200.f, 300.f},
    };
    level3.coins = {
        {{50.f, 450.f}},
        {{150.f, 350.f}},
        {{250.f, 250.f}},
    };
    level3.enemies = {
        {{300.f, 500.f}},
        {{400.f, 400.f}},
    };
    level3.weapons = {
        {{500.f, 500.f}, "weapon1.png"},
        {{600.f, 400.f}, "weapon2.png"},
    };
    levels.push_back(level3);
    //Load level 4
    Level level4;
    level4.platforms = {
        {0.f, 500.f},
        {100.f, 400.f},
        {200.f, 300.f},
    };
    level4.coins = {
        {{50.f, 450.f}},
        {{150.f, 350.f}},
        {{250.f, 250.f}},
    };
    level4.enemies = {
        {{300.f, 500.f}},
        {{400.f, 400.f}},
    };
    level4.weapons = {
        {{500.f, 500.f}, "weapon1.png"},
        {{600.f, 400.f}, "weapon2.png"},
    };
    levels.push_back(level4);
    //Load level 5
    Level level5;
    level5.platforms = {
        {0.f, 500.f},
        {100.f, 400.f},
        {200.f, 300.f},
    };
    level5.coins = {
        {{50.f, 450.f}},
        {{150.f, 350.f}},
        {{250.f, 250.f}},
    };
    level5.enemies = {
        {{300.f, 500.f}},
        {{400.f, 400.f}},
    };
    level5.weapons = {
        {{500.f, 500.f}, "weapon1.png"},
        {{600.f, 400.f}, "weapon2.png"},
    };
    levels.push_back(level5);
    //Load level 6
    Level level6;
    level6.platforms = {
        {0.f, 500.f},
        {100.f, 400.f},
        {200.f, 300.f},
    };
    level6.coins = {
        {{50.f, 450.f}},
        {{150.f, 350.f}},
        {{250.f, 250.f}},
    };
    level6.enemies = {
        {{300.f, 500.f}},
        {{400.f, 400.f}},
    };
    level6.weapons = {
        {{500.f, 500.f}, "weapon1.png"},
        {{600.f, 400.f}, "weapon2.png"},
    };
    levels.push_back(level6);
    
}


    sf::Sprite backgroundSprite;
    backgroundSprite.setTexture(backgroundTexture);
    backgroundSprite.setPosition(0, 0);

    // Game loop
    while (window.isOpen())
    {
        // Handle events
        sf::Event event;
        while (window.pollEvent(event))
        {
            if (event.type == sf::Event::Closed)
            {
                window.close();
            }
        }

        // Move player sprite
        //Move Backwards
        if (sf::Keyboard::isKeyPressed(sf::Keyboard::Left))
        {
            playerSprite.move(-5, 0);
        }
        //Move Forward
        if (sf::Keyboard::isKeyPressed(sf::Keyboard::Right))
        {
            playerSprite.move(5, 0);
        }
        //Move Up
        if (sf::Keyboard::isKeyPressed(sf::Keyboard::Up))
        {
            playerSprite.move(0, -5);
        }
        //Move Down
        if (sf::Keyboard::isKeyPressed(sf::Keyboard::Down))
        {
            playerSprite.move(0, 5);
        }
        //Jump Command
        if (sf::Keyboard::isKeyPressed(sf::Keyboard::Space))
        {
            playerSprite.jump(0, -5);
        }
        //Double Jump Command
        if(sf::Keyboard::isKeyPressed(sf::Keyboard::Double Space))
        {
            playerSprite.Double jump(0,-10);
        }
        //Sprint Backwards Command
        if(sf::Keyboard::isKeyPressed(sf::Keyboard::left::isKeyPressed O))
        {
            playerSprite.Sprint move left(-10,0);
        }
        //Sprint Forwards Command
        if(sf::Keyboard::isKeyPressed(sf::Keyboard::right::isKeyPressed P))
        {
            playerSprite.Sprint move right(10,0);
        }
        // Load Textures and Sprites for Mario, Background, and Coins
        sf::Texture marioTexture;
        if (!marioTexture.loadFromFile("mario.png")) {
          std::

        
        //Stores enemies
        std::vector<Enemy> enemies;
        
        // Clear the window
        window.clear();

        // Draw the background
        window.draw(backgroundSprite);

        // Draw the player
        window.draw(playerSprite);

        // Display the window
        window.display();
    }

    return 0;
}



