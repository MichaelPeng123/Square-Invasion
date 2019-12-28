# Square Invasion
	My game created with sfml.
	#include <sstream>
	#include <iostream>
	#include <vector>
	#include <SFML/Graphics.hpp>
	using namespace std;
	int main()
	{
		int count = 0;
		int abilitycooldown = 0;
		bool canuseability = true;
		int dashT = 0;
		int regulator = 2000;
		int spawnlocation = 0;
		int dashregulator = 0;
		int abilityC = 0;
		bool dashchecker = false;
		sf::Texture ded;
		ded.loadFromFile("ability1.png");
		sf::Texture enemyTexture;
		enemyTexture.loadFromFile("enemy.png");
		sf::Texture playerTexture;
		playerTexture.loadFromFile("player.png");
		sf::Texture bulletTexture1;
		bulletTexture1.loadFromFile("laserTop.png");
		sf::Texture bulletTexture2;
		bulletTexture2.loadFromFile("laserRight.png");

		sf::Sprite ability1;
		ability1.setTexture(ded);
		sf::Sprite enemy;
		sf::Sprite PlayerDot;
		PlayerDot.setTexture(playerTexture);
		sf::Sprite bullet1;
		sf::Sprite bullet2;
		sf::Sprite bullet3;
		sf::Sprite bullet4;

		ability1.scale(2, 2);
		ability1.setPosition(-500, -500);
		PlayerDot.setOrigin(30, 30);
		PlayerDot.setScale(0.15, 0.15);
		PlayerDot.setPosition(480, 480);
		int mousex = 0;
		int mousey = 0;
		bool playerdash = false;
		int playerdashregulator = 4000;
		int dashx = 0;
		int dashy = 0;
		int dashchangex = 0;
		int dashchangey = 0;
		vector<sf::Sprite> enemiesVector;
		vector<bool> enemiesbool;
		vector<sf::Text> textV;
		vector<float> enemiesHeight;
		vector<float> enemiesWidth;
		float laserHeight1 = -1;
		float laserWidth1 = -1;
		float laserHeight2 = -1;
		float laserWidth2 = -1;
		float laserHeight3 = -1;
		float laserWidth3 = -1;
		float laserHeight4 = -1;
		float laserWidth4 = -1;
		bool laser1 = false;
		bool laser2 = false;
		bool laser3 = false;
		bool laser4 = false;
		int lives = 20;
		sf::Font font;
		font.loadFromFile("fontfile.ttf");
		int score = 0;
		std::ostringstream ssScore;
		ssScore << "Score " << score;
		sf::Text text;
		int duringdashcounter = 0;
		text.setFont(font);
		text.setString(ssScore.str());
		sf::RenderWindow window(sf::VideoMode(1000, 1000), "Display");
		bool cooldown = true;
		int cool = 0;
		sf::Texture bossTexture;
		bossTexture.loadFromFile("boss.png");
		sf::Sprite boss;
		boss.setTexture(bossTexture);
		int bosscounter = 0;
		int bosshealth;  
		bool bossalive = false;
		enemiesVector.push_back(enemy);
		enemiesbool.push_back(false);
		bool start = true;
		while (window.isOpen()) {
			if (!start) {
				if (lives > 0) {
					sf::Event event;
					while (window.pollEvent(event)) {
						if (event.type == sf::Event::Closed) {
							window.close();
							break;
						}
					}
					if (playerdash == false) {
						if (sf::Keyboard::isKeyPressed(sf::Keyboard::Key::A)) {
							if (PlayerDot.getPosition().x >= -10)
								PlayerDot.move(-0.24, 0.0);
						}
						if (sf::Keyboard::isKeyPressed(sf::Keyboard::Key::S)) {
							if (PlayerDot.getPosition().y <= 973)
								PlayerDot.move(0.0, 0.24);
						}
						if (sf::Keyboard::isKeyPressed(sf::Keyboard::Key::D)) {
							if (PlayerDot.getPosition().x <= 973)
								PlayerDot.move(0.24, 0.0);
						}
						if (sf::Keyboard::isKeyPressed(sf::Keyboard::Key::W)) {
							if (PlayerDot.getPosition().y >= -10)
								PlayerDot.move(0.0, -0.24);
						}
					}
					if (sf::Mouse::isButtonPressed(sf::Mouse::Right) && canuseability == true) {
						abilityC = 0;
						abilitycooldown = 0;
						canuseability = false;
						ability1.setPosition(PlayerDot.getPosition().x - 30, PlayerDot.getPosition().y - 30);
					}
					if (sf::Keyboard::isKeyPressed(sf::Keyboard::Key::P)) {
						lives = 0;
					}

					if (sf::Keyboard::isKeyPressed(sf::Keyboard::Key::Space) && cooldown == true) {
						laserHeight1 = PlayerDot.getPosition().y;
						laserWidth1 = PlayerDot.getPosition().x;
						laserHeight2 = PlayerDot.getPosition().y;
						laserWidth2 = PlayerDot.getPosition().x;
						laserHeight3 = PlayerDot.getPosition().y;
						laserWidth3 = PlayerDot.getPosition().x;
						laserHeight4 = PlayerDot.getPosition().y;
						laserWidth4 = PlayerDot.getPosition().x;
						laser1 = true;
						laser2 = true;
						laser3 = true;
						laser4 = true;
						cooldown = false;
						cool = 0;
					}
					bool test;
					if (sf::Mouse::isButtonPressed(sf::Mouse::Left) && playerdashregulator >= 4000) {
						sf::Vector2i mousePos = sf::Mouse::getPosition(window);
						if (PlayerDot.getPosition().x > mousePos.x)
							test = true;
						else
							test = false;
						playerdashregulator = 0;
						playerdash = true;
						dashT;
						mousex = (float)mousePos.x;
						mousey = (float)mousePos.y;
						dashchangex = (mousex - PlayerDot.getPosition().x) * 0.2;
						dashchangey = (mousey - PlayerDot.getPosition().y) * 0.2;
					}
					cool++;
					if (cool == 2000) {
						cooldown = true;
						cool = 0;
					}
					abilitycooldown++;
					if (abilitycooldown >= 3000) {
						canuseability = true;
					}
					if (regulator == 2000) {
						regulator = 0;
						if (spawnlocation == 0) {
							enemiesHeight.push_back(-30);
							enemiesWidth.push_back(-30);
						}
						else if (spawnlocation == 1) {
							enemiesHeight.push_back(-30);
							enemiesWidth.push_back(1030);
						}
						else if (spawnlocation == 2) {
							enemiesHeight.push_back(1030);
							enemiesWidth.push_back(1030);
						}
						else {
							enemiesHeight.push_back(1030);
							enemiesWidth.push_back(-30);
							spawnlocation = 0;
						}
						spawnlocation++;
					}
					if (laser1)
						laserHeight1 += -1;
					if (laser2)
						laserHeight2 += 1;
					if (laser3)
						laserWidth3 += 1;
					if (laser4)
						laserWidth4 += -1;

					if (laserHeight1 < 0)
						laser1 = false;
					if (laserHeight2 > 1000)
						laser2 = false;
					if (laserWidth3 > 1000)
						laser3 = false;
					if (laserWidth4 < 0)
						laser4 = false;

					for (int i = 0; i < enemiesVector.size() - 1; i++) {
						if (enemiesVector[i].getGlobalBounds().intersects(bullet1.getGlobalBounds())) {

							enemiesVector.erase(enemiesVector.begin() + i);
							enemiesHeight.erase(enemiesHeight.begin() + i);
							enemiesWidth.erase(enemiesWidth.begin() + i);
							//laser1 = false;
							score += 1000;
							ssScore.str("");
							ssScore << "Score  " << score;
							text.setString(ssScore.str());
							break;
						}
						if (enemiesVector[i].getGlobalBounds().intersects(bullet2.getGlobalBounds())) {

							enemiesVector.erase(enemiesVector.begin() + i);
							enemiesHeight.erase(enemiesHeight.begin() + i);
							enemiesWidth.erase(enemiesWidth.begin() + i);
							//laser2 = false;
							score += 1000;
							ssScore.str("");
							ssScore << "Score  " << score;
							text.setString(ssScore.str());
							break;
						}
						if (enemiesVector[i].getGlobalBounds().intersects(bullet3.getGlobalBounds())) {

							enemiesVector.erase(enemiesVector.begin() + i);
							enemiesHeight.erase(enemiesHeight.begin() + i);
							enemiesWidth.erase(enemiesWidth.begin() + i);
							//laser3 = false;
							score += 1000;
							ssScore.str("");
							ssScore << "Score  " << score;
							text.setString(ssScore.str());
							break;
						}
						if (enemiesVector[i].getGlobalBounds().intersects(bullet4.getGlobalBounds())) {

							enemiesVector.erase(enemiesVector.begin() + i);
							enemiesHeight.erase(enemiesHeight.begin() + i);
							enemiesWidth.erase(enemiesWidth.begin() + i);
							//laser4 = false;
							score += 1000;
							ssScore.str("");
							ssScore << "Score  " << score;
							text.setString(ssScore.str());
							break;
						}
						if (enemiesVector[i].getGlobalBounds().intersects(PlayerDot.getGlobalBounds())) {
							lives--;
							enemiesVector.erase(enemiesVector.begin() + i);
							enemiesHeight.erase(enemiesHeight.begin() + i);
							enemiesWidth.erase(enemiesWidth.begin() + i);
							continue;
						}
						if (enemiesVector[i].getGlobalBounds().intersects(ability1.getGlobalBounds())) {
							enemiesVector.erase(enemiesVector.begin() + i);
							enemiesHeight.erase(enemiesHeight.begin() + i);
							enemiesWidth.erase(enemiesWidth.begin() + i);
							score += 1000;
							ssScore.str("");
							ssScore << "Score  " << score;
							text.setString(ssScore.str());
						}
						int distance = (PlayerDot.getPosition().x - enemiesWidth[i]) * (PlayerDot.getPosition().x - enemiesWidth[i])
							+ (PlayerDot.getPosition().y - enemiesHeight[i]) * (PlayerDot.getPosition().y - enemiesHeight[i]);
						if (distance != 0) {
							int change = 20000000 / distance;
							enemiesWidth[i] = enemiesWidth[i] - (change * (enemiesWidth[i] - PlayerDot.getPosition().x) * 0.0000005);
							enemiesHeight[i] = enemiesHeight[i] - (change * (enemiesHeight[i] - PlayerDot.getPosition().y) * 0.0000005);
						}
						enemiesWidth[i] = enemiesWidth[i] - ((enemiesWidth[i] - PlayerDot.getPosition().x) * 0.0002);
						enemiesHeight[i] = enemiesHeight[i] - ((enemiesHeight[i] - PlayerDot.getPosition().y) * 0.0002);
					}
					sf::Text live;
					std::ostringstream sslives;
					live.setFont(font);
					sslives << "Lives  " << lives;
					live.setString(sslives.str());

					live.setPosition(870, 0);

					if (playerdash == true) {
						dashT++;
						if (PlayerDot.getPosition().x != mousex && PlayerDot.getPosition().y != mousey && dashT < 300) {
							PlayerDot.move(dashchangex * 0.01, dashchangey * 0.01);
						}

						else {
							playerdash = false;
							dashT = 0;
						}
					}
					if (boss.getGlobalBounds().intersects(ability1.getGlobalBounds())) {
						count++;
						boss.setPosition(-50, -50);
						ability1.setPosition(-100, -100);
						if (count == 1) {
							score += 10000;
							ssScore.str("");
							ssScore << "Score  " << score;
							text.setString(ssScore.str());
						}
						bossalive = false;
					}
					//WINDOW CLEAR
					//WINDOW CLEAR
					//WINDOW CLEAR
					//WINDOW CLEAR
					window.clear();
					window.draw(text);
					enemiesVector.clear();
					window.draw(live);


					if (bosscounter == 20000) {
						bossalive = true;
						boss.setPosition(-100, -100);
						bosshealth = 5;
						count = 0;
						bosscounter = 0;
					}
					float x = boss.getPosition().x - PlayerDot.getPosition().x;
					float y = boss.getPosition().y - PlayerDot.getPosition().y;
					float distance = 500 / (sqrt((x*x + y * y)));
					if (bossalive == true) {
						if (dashregulator == 4000) {
							dashregulator = 0;
							dashchecker = true;
							duringdashcounter = 0;
							float x = boss.getPosition().x - PlayerDot.getPosition().x;
							float y = boss.getPosition().y - PlayerDot.getPosition().y;
							distance = 500 / (sqrt((x*x + y * y)));
							dashx = boss.getPosition().x - PlayerDot.getPosition().x;
							dashy = boss.getPosition().y - PlayerDot.getPosition().y;
						}
						if (dashchecker = true) {
							if (duringdashcounter >= 1000) {
								dashchecker = false;
							}
							else {
								boss.move(-distance * dashx*0.0008, -distance * dashy*0.0008);
								duringdashcounter++;
							}
						}
						if (boss.getGlobalBounds().intersects(PlayerDot.getGlobalBounds())) {
							lives = 0;
						}
						if (boss.getGlobalBounds().intersects(bullet4.getGlobalBounds())) {
							bosshealth--;
							laser4 = false;
						}
						if (boss.getGlobalBounds().intersects(bullet3.getGlobalBounds())) {
							bosshealth--;
							laser3 = false;
						}
						if (boss.getGlobalBounds().intersects(bullet2.getGlobalBounds())) {
							bosshealth--;
							laser2 = false;
						}
						if (boss.getGlobalBounds().intersects(bullet1.getGlobalBounds())) {
							bosshealth--;
							laser1 = false;
						}
						if (dashchecker == false)
							boss.move((boss.getPosition().x - PlayerDot.getPosition().x) * -0.0002, (boss.getPosition().y - PlayerDot.getPosition().y) * -0.0002);
						if (bosshealth == 0) {
							dashregulator = 0;
							score += 10000;
							ssScore.str("");
							ssScore << "Score " << score;
							text.setString(ssScore.str());
							bossalive = false;
							boss.setPosition(-50, -50);
						}
					}
					enemiesbool.clear();
					for (int i = 0; i < enemiesWidth.size(); i++) {
						enemy.setTexture(enemyTexture);
						enemy.setPosition(enemiesWidth[i], enemiesHeight[i]);
						window.draw(enemy);
						enemiesVector.push_back(enemy);
					}
					if (abilityC < 3000) {
						window.draw(ability1);
						abilityC++;
					}
					else {
						ability1.setPosition(-500, -500);
					}
					if (laser1) {
						bullet1.setTexture(bulletTexture1);
						bullet1.setPosition(laserWidth1, laserHeight1);
						window.draw(bullet1);
					}
					else {
						bullet1.setPosition(-500, -500);
					}
					if (laser2) {
						bullet2.setTexture(bulletTexture1);
						bullet2.setPosition(laserWidth2, laserHeight2);
						window.draw(bullet2);
					}
					else {
						bullet2.setPosition(-500, -500);
					}

					if (laser3) {
						bullet3.setTexture(bulletTexture2);
						bullet3.setPosition(laserWidth3, laserHeight3);
						window.draw(bullet3);
					}
					else {
						bullet3.setPosition(-500, -500);
					}

					if (laser4) {
						bullet4.setTexture(bulletTexture2);
						bullet4.setPosition(laserWidth4, laserHeight4);
						window.draw(bullet4);
					}
					else {
						bullet4.setPosition(-500, -500);
					}
					if (bossalive == true) {
						window.draw(boss);
					}
					window.draw(PlayerDot);
					window.display();
					if (abilitycooldown == false)
						ability1.setPosition(-500, -500);
					regulator++;
					if (bossalive == false) {
						bosscounter++;
					}
					else {
						dashregulator++;
					}
					if (playerdash == false) {
						playerdashregulator++;
					}
				}
				else {
					window.clear();
					sf::Text done;
					done.setFont(font);
					std::ostringstream sslives;
					sslives << "                           GAME  OVER\n                 YOUR  SCORE  IS\n                                   " << score << "\n PRESS  SPACE  TO  CONTINUE";
					done.setString(sslives.str());
					done.setCharacterSize(40);
					done.setPosition(271, 400);
					if (sf::Keyboard::isKeyPressed(sf::Keyboard::Key::Space))
						window.close();
					window.draw(done);
					window.display();
				}
			}
			else {
			sf::Text startT;
			startT.setFont(font);
			startT.setString("Press  Enter  To  Start");
			window.clear();
			startT.setPosition(300, 500);
			window.draw(startT);
			window.display();
			if (sf::Keyboard::isKeyPressed(sf::Keyboard::Key::Enter))
				start = false;
			}
		}
		return 0;
	}
