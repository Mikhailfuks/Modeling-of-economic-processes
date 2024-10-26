#include <iostream>
#include <vector>
#include <random>
#include <cmath>

using namespace std;

// Структура для представления товара
struct Good {
    string name; // Название товара
    double price; // Цена товара
    int supply;   // Предложение товара
    int demand;   // Спрос на товар
};

// Класс для моделирования рынка
class Market {
private:
    vector<Good> goods; // Список товаров на рынке
    random_device rd;
    mt19937 gen;
    uniform_real_distribution<> dist;

public:
    // Конструктор
    Market() : gen(rd()), dist(0.0, 1.0) {}

    // Добавление товара на рынок
    void addGood(const string& name, double initialPrice) {
        goods.push_back({name, initialPrice, 0, 0});
    }

    // Моделирование спроса и предложения
    void simulateDemandSupply() {
        for (auto& good : goods) {
            // Случайная генерация спроса и предложения
            good.supply = int(dist(gen) * 100); // Предложение
            good.demand = int(dist(gen) * 100); // Спрос

            // Изменение цены в зависимости от соотношения спроса и предложения
            if (good.supply > good.demand) {
                good.price *= 0.95; // Снижение цены
            } else if (good.supply < good.demand) {
                good.price *= 1.05; // Повышение цены
            }
        }
    }

    // Вывод информации о рынке
    void printMarketInfo() const {
        cout << "Информация о рынке:\n";
        for (const auto& good : goods) {
            cout << "Товар: " << good.name << "\n";
            cout << "Цена: " << good.price << "\n";
            cout << "Предложение: " << good.supply << "\n";
            cout << "Спрос: " << good.demand << "\n";
            cout << "------------------------\n";
        }
    }
};

int main() {
    // Создание рынка
    Market market;

    // Добавление товаров на рынок
    market.addGood("Хлеб", 10.0);
    market.addGood("Молоко", 5.0);
    market.addGood("Яблоки", 2.0);

    // Моделирование рынка в течение 10 раундов
    for (int i = 0; i < 10; ++i) {
        cout << "Раунд " << i + 1 << ":\n";
        market.simulateDemandSupply();
        market.printMarketInfo();
        cout << "------------------------\n";
    }

    return 0;
}
