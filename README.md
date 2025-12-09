#include <iostream>
#include <windows.h>
#include <ctime>
#include <iomanip>
#include <string>
#include <unordered_set>

//================= Учётки ================

size_t userSize = 3;
size_t staffCount = 1;
std::string userStatus[3]{ "Супер Администратор","Администратор","Пользователь" };

std::string* loginArr = new std::string[userSize]{ "admin", "adminOne","user" };
std::string* passArr = new std::string[userSize]{ "admin","adminOne","user" };
std::string* statusArr = new std::string[userSize]{ userStatus[0],userStatus[1], userStatus[2] };
double* salesArr = new double[userSize] {0.0, 0.0};
unsigned int* userIdArr = new unsigned int[userSize] {1, 2};
unsigned int currentId = 0;
std::string currentStatus;
void CheckArrPushback();
void ChangeAccount();
void PrintCheck(double& totalSum);

//======================================================



//====================== Склад =========================

size_t storageSize = 10;
size_t maxItemSize = 299;
unsigned int* idArr = nullptr;
std::string* nameArr = nullptr;
double* priceArr = nullptr;
unsigned int* coutArr = nullptr;
bool isStorageCreated = false;
std::unordered_set<char> specialSymbols;

//======================================================



//===================== Меню ======================

std::unordered_set<char> passSymbols;
bool isPassSetCreated = false;
bool isSetCreated = false;
void CreateStorage();
void CreateNewStorage();
void ShowStorage(int mode = 0);
void AddStorageItem();
void ShowSuperAdminMenu();
void ShowAdminMenu();
void ShowuserMenu();
bool IsNumber(const std::string& str);
inline void Getline(std::string& str);
void Start();
bool Login();
bool CheckPass(const std::string& str);
void RemoveStorageItem();
void ChangePrice();
void ChangeStorage();
void AddNewItem();
void ChandeName();
void DeleteItem();
void ShowUsers(int mode = 0);
void AddNewUser();
bool CheckLogin(const std::string& str);
void SetSpecialSymbols();
void ChangeAccount();
void SetPassSymbols();
void AddNewUser();
void ChangePass();
void DeleteUser();
void StorageReturner();
void ShowIncome();
template<typename ArrType>
void FillArr(ArrType* dynamicArr, ArrType* staticArr, size_t arraySize);
size_t checkSize = 0;
int* idArrCheck;
std::string* nameArrCheck;
unsigned int* countArrCheck;
double* priceArrCheck;
double* totalPriceArrCheck;

double cashIncome = 0.0;
double bankIncome = 0.0;
double cash = 30000 + rand() % 700;

void Selling();

//======================================================



//======================== Мэйн ==============================

inline void Getline(std::string& str);
inline void Err();
int main()
{
	SetConsoleCP(1251);
	SetConsoleOutputCP(1251);
	srand(time(NULL));

	Start();
	delete[]loginArr, passArr, statusArr;
	if (isStorageCreated)
	{
		delete[]idArr, nameArr, coutArr, priceArr;
	}
	return 0;
}
void Selling()
{
	std::string chooseId, chooseCount, chooseMoney, choose;
	unsigned int id = 0, count = 0, index = -1;
	double money = 0.0, totalSum = 0.0;
	bool isFirst = false;
	checkSize = 0;
	while (true)
	{
		ShowStorage();

		std::cout << "Введите id товара для покупки или\"exit\" для завершения покупки: ";
		Getline(chooseId);
		if (chooseId == "exit")
		{
			system("cls");
			if (isFirst == false)
			{
				std::cout << "Отмена покупки!\n";
				Sleep(1500);
				break;
			}
			PrintCheck(totalSum);
			std::cout << "\nПодтвердить покупку?\n1 - Да\n2 - Продолжить покупки\n3 - Отмена\nВыбор: ";
			if (choose == "1")
			{
				while (true)
				{
					system("cls");
					std::cout << "Выберите способ оплаты\n1 - Наличные\n1 - Безналичные\nВыбор: ";
					Getline(choose);
					if (choose == "1")
					{
						std::cout << "Введите кол-во наличных: ";
						Getline(chooseMoney);
						if (IsNumber(chooseMoney))
						{
							money = std::stod(chooseMoney);
							if (money < totalSum)
							{
								std::cout << "Недостаточно денег!\n";
								Sleep(1500);
								continue;
							}
							else if (money - totalSum > cash)
							{
								std::cout << "У нас недостаточно сдачи, попробуйте карту!\n";
								Sleep(1500);
								continue;
							}
							else
							{
								std::cout << "Сумма: " << money << "\n";
								Sleep(400);
								std::cout << "Спасибо за покупку! Ваша сдача: " << money - totalSum << " рублей\n";
								Sleep(2000);
								cash += totalSum;
								cash -= money - totalSum;
								cashIncome += totalSum;
								salesArr[currentId] += totalSum;
								system("cls");
								break;
							}
						}
					}
					else if (choose == "2")
					{
						std::cout << "Обработка платежа...\n\n";
						Sleep(1000);
						if (rand() % 10 <= 2)
						{
							for (size_t i = 0; i < 5; i++)
							{
								std::cout << i + 1 << "\t";
								Sleep(500);
							}
							std::cout << "\nПлатеж не прошел\n\nПопробуйте снова!";
							bankIncome += totalSum;
							salesArr[currentId] += totalSum;
							system("cls");
							Sleep(1500);
							break;

						}
					}
					else if (choose == "crblrf")
					{
						std::cout << "Промокод активирован! Счет обнулен\n";
						Sleep(1500);
						system("cls");
						break;
					}
					else
					{
						Err();
					}

				}
			}
			else if (choose == "2")
			{
				continue;
			}
			else if (choose == "3")
			{
				std::cout << "Покупка отменена!\n";
				void StorageReturner();
				Sleep(1500);
				return;
			}
			else
			{
				Err();
				continue;
			}
			system("pause");
			delete[] idArrCheck;
			delete[] nameArrCheck;
			delete[] countArrCheck;
			delete[] priceArrCheck;
			delete[] totalPriceArrCheck;
			idArrCheck = nullptr;
			nameArrCheck = nullptr;
			countArrCheck = nullptr;
			priceArrCheck = nullptr;
			totalPriceArrCheck = nullptr;

			break;
		}

		if (IsNumber(chooseId))
		{
			id = std::stoi(chooseId) - 1;
			if (id < 0 || id > storageSize - 1)
			{
				std::cout << "Неверный id\n";
				Sleep(1500);
				continue;
			}
		}
		else
		{
			continue;
		}
		std::cout << "\nВведите кол-во товара или \"exit\" для отмены покупки: ";
		Getline(chooseCount);
		if (chooseCount == "exit")
		{
			std::cout << "Отмена выбора товара: " << nameArr[id] << "\n\n";
			Sleep(1500);
			continue;
		}
		if (IsNumber(chooseCount))
		{
			count = std::stoi(chooseCount) - 1;
			if (count < 1 || count > coutArr[id])
			{
				std::cout << "Неверное кол-во! Доступно: " << coutArr[id] << "\n\n";
				Sleep(1500);
				continue;
			}
		}
		else
		{
			Err();
			continue;
		}


		CheckArrPushback();
		if (isFirst == false)
		{
			isFirst = true;
		}
		index++;
		idArrCheck[index] = idArr[id];
		nameArrCheck[index] = nameArr[id];
		priceArrCheck[index] = priceArr[id];
		countArrCheck[index] = count;
		totalPriceArrCheck[index] = count * priceArr[id];
		coutArr[id] -= count;
		totalSum += totalPriceArrCheck[index];

		std::cout << "\nТовар добавлен в чек\n\n";

		Sleep(1000);
	}
}
void PrintCheck(double& totalSum)
{
	std::cout << "№\t" << "ID\t" << std::left << std::setw(25) << "Название товара\t\t"
		<< "Цена за шт\t" << "Кол-во\n" << "Итого\n";
	for (size_t i = 0; i < checkSize; i++)
	{
		std::cout << i + 1 << "\t" << idArrCheck[i] << "\t" << std::left << std::setw(25) << nameArrCheck[i] << "\t"
			<< priceArrCheck[i] << "\t\t" << countArrCheck[i] << "\t" << totalPriceArrCheck[i] << "\n";
	}
	std::cout << "\nИтого к оплате: " << totalSum << "\n\n";
}
void CheckArrPushback()
{
	checkSize++;
	int* idArrCheckTemp = new int[checkSize];
	std::string* nameArrCheckTemp = new std::string[checkSize];
	unsigned int* countArrCheckTemp = new unsigned int[checkSize];
	double* priceArrCheckTemp = new double[checkSize];
	double* totalPriceArrCheckTemp = new double[checkSize];

	FillArr(idArrCheckTemp, idArrCheck, checkSize - 1);
	FillArr(nameArrCheckTemp, nameArrCheck, checkSize - 1);
	FillArr(countArrCheckTemp, countArrCheck, checkSize - 1);
	FillArr(priceArrCheckTemp, priceArrCheck, checkSize - 1);
	FillArr(totalPriceArrCheckTemp, totalPriceArrCheck, checkSize - 1);
	std::swap(idArrCheckTemp, idArrCheck);
	std::swap(nameArrCheckTemp, nameArrCheck);
	std::swap(countArrCheckTemp, countArrCheck);
	std::swap(priceArrCheckTemp, priceArrCheck);
	std::swap(totalPriceArrCheckTemp, totalPriceArrCheck);


	delete[] idArrCheckTemp;
	delete[] nameArrCheckTemp;
	delete[] countArrCheckTemp;
	delete[] priceArrCheckTemp;
	delete[] totalPriceArrCheckTemp;

}
void ChangePrice()
{
	std::string chooseId, choosePrice, choose;
	unsigned int id = 0;
	double newPrice;
	while (true)
	{
		system("cls");
		ShowStorage(2);
		std::cout << "Введите id товара или \"exit\" для выхода";
		Getline(chooseId);
		if (chooseId == "exit")
		{

			std::cout << "Отмена изменения цены!\n";
			Sleep(1500);
			system("cls");
			break;

		}
		std::cout << "Введите новую цену или отмена";
		Getline(choosePrice);

		if (IsNumber(chooseId) && IsNumber(choosePrice))
		{
			id = std::stoi(chooseId) - 1;
			newPrice = std::stoi(choosePrice);

			if (id < 0 || id > storageSize - 1 || newPrice < 0 || newPrice > 10000000)
			{
				std::cout << "Некорректный id или цена\nМаксимальная цена - " << 10000000 << "\n\n";
				Sleep(1500);

			}
			else
			{
				std::cout << std::left << std::setw(25) << nameArr[id] << "\t"
					<< coutArr[id] << " ---> " << coutArr[id] + newPrice << "\n\n";
				std::cout << "Подтвердить?\n1 - Да\n2 - Нет\nВыбор: ";
				Getline(choose);
				if (choose == "1")
				{
					coutArr[id] += newPrice;
					std::cout << "Цена успешно изменена\n\n";
					Sleep(1500);
					system("cls");
					break;


				}
				else if (choose == "2")
				{
					std::cout << "Отмена изменения цены!\n";
					Sleep(1500);
					system("cls");
				}
				else
				{
					Err();
				}
			}


		}


	}

}
void SetSpecialSymbols()
{
	for (char i = '0'; i <= '9'; i++)
	{
		specialSymbols.insert(i);
	}
	for (char i = '0'; i <= 'Z'; i++)
	{
		specialSymbols.insert(i);
	}
	for (char i = '0'; i <= 'z'; i++)
	{
		specialSymbols.insert(i);
	}
	isSetCreated = true;
}
void SetPassSymbols()
{
	for (char i = '!'; i <= '&'; i++)
	{
		specialSymbols.insert(i);
	}
	for (char i = '('; i <= '+'; i++)
	{
		specialSymbols.insert(i);
	}
	for (char i = '/'; i <= '~'; i++)
	{
		specialSymbols.insert(i);
	}
	isPassSetCreated = true;
}
bool CheckPass(const std::string& str)
{
	if (str.size() < 8 || str.size() > 30)
	{
		std::cout << "Некорректная длина пароля\n";
		Sleep(1500);
		return false;
	}
	int numCount = 0, symCount = 0;
	std::unordered_set<char> specialPassSym{ '!', '@', '#', '$', '%', '^', '&', '*', '=', '+', '/', '?', '|', '\\', '\"', '\'', ',', '.', '<', '>', '`', ':', ';', };
	for (char sym : str)
	{
		if (!passSymbols.count(sym))
		{
			std::cout << "Недопустимый символ\n";
			Sleep(1500);
			return false;
		}
		if (std::isdigit(sym))
		{
			numCount++;
		}
		if (specialPassSym.count(sym))
		{
			symCount++;
		}
	}

	if (numCount > 2 && symCount > 3)
	{
		return true;
	}
	else
	{
		std::cout << "Нужно 3 цифры и 3 символа\n";
		return false;
	}
}
void AddNewUser()
{
	std::string newLogin, newPass, newRole, choose;
	bool exit = true;
	while (true)
	{

		while (true)
		{
			system("cls");
			std::cout << "\nВведите логин нового пользователя или \"exit\" для отмены: ";
			Getline(newLogin);
			if (newLogin == "exit")
			{
				std::cout << "Отмена создания пользователя!\n";
				Sleep(1500);
				exit = false;
				break;
			}
			if (CheckLogin(newLogin))
			{
				break;
			}
			else
			{
				std::cout << "Допустимы символы a-z, A-z, 0-9\n\n";
				Sleep(1500);
			}
		}
		while (exit)
		{
			system("cls");
			std::cout << "\nВведите пароль нового пользователя или \"exit\" для отмены: ";
			Getline(newPass);
			if (newPass == "exit")
			{
				std::cout << "Отмена создания пользователя!\n";
				Sleep(1500);
				exit = false;
				break;
			}
			if (CheckLogin(newPass))
			{
				break;
			}
			else
			{
				std::cout << "Допустимы символы a-z, A-z, 0-9\n\n";
				Sleep(1500);
			}
		}
		while (exit)
		{
			system("cls");
			std::cout << "\nВведите роль нового пользователя или \"exit\" для отмены: ";
			std::cout << "1 - Администратор\n2 - Пользователь\nВыбор: ";
			Getline(newPass);
			if (newPass == "exit")
			{
				Getline(choose);
				if (choose == "exit")
				{
					std::cout << "Отмена создания пользователя!\n";
				}
				Sleep(1500);
				exit = false;
				break;
			}
			if (choose == "1")
			{
				newRole = userStatus[1];
				break;
			}
			else if (choose == "2")
			{
				newRole = userStatus[2];
				break;
			}
			else
			{
				Err();
			}
		}
		while (exit)
		{
			std::cout << "Пользователь: " << newLogin << "\n";
			std::cout << "Роль: " << newRole << "\n\n";
			std::cout << "Подтвердить?\n1 - Да\n2- Нет\nВыбор: ";
			Getline(choose);
			if (choose == "1")
			{
				userSize++;
				if (newRole == userStatus[2])
				{
					staffCount++;
				}
				std::string* loginArrTemp = new std::string[userSize];
				std::string* passArrTemp = new std::string[userSize];
				std::string* statusArrTemp = new std::string[userSize];
				double* salesArrTemp = new double[userSize];
				unsigned int* userIdArrTemp = new unsigned int[userSize];
				FillArr(loginArrTemp, loginArr, userSize);
				FillArr(passArrTemp, passArr, userSize);
				FillArr(statusArrTemp, statusArr, userSize);
				FillArr(salesArrTemp, salesArr, userSize);
				FillArr(userIdArrTemp, userIdArr, userSize);
				loginArrTemp[userSize - 1] = newLogin;
				passArrTemp[userSize - 1] = newPass;
				statusArrTemp[userSize - 1] = newRole;

				std::swap(loginArrTemp, loginArr);
				std::swap(passArrTemp, passArr);
				std::swap(statusArrTemp, statusArr);
				std::swap(salesArrTemp, salesArr);
				std::swap(userIdArrTemp, userIdArr);
				delete[] loginArrTemp, passArrTemp, statusArrTemp, salesArrTemp, userIdArrTemp;
				std::cout << "Загрузка...";
				Sleep(2000);
				std::cout << "Пользователь успешно создан!\n\n";
				Sleep(1500);
				break;
			}
			else if (choose == "2")
			{
				std::cout << "Отмена\n";
				Sleep(1500);
				break;
			}
			else
			{
				Err();
			}

		}
		if (exit == false)
		{
			break;
		}
	}
}
void ChangeStorage()
{
	std::string choose;
	while (true)
	{
		system("cls");
		std::cout << "1 - Добавить новый товар\n";
		std::cout << "2 - Изменить название товара\n";
		std::cout << "3 - Удалить товар\n";
		std::cout << "0 - Выйти в главное меню\n";
		Getline(choose);
		if (choose == "1" && storageSize > 0)
		{
			AddNewItem();
		}
		else if (choose == "2" && storageSize > 0)
		{
			ChandeName();
		}
		else if (choose == "3" && storageSize > 0)
		{
			DeleteItem();
		}
		else if (choose == "0" && storageSize > 0)
		{
			system("cls");
			break;
		}
		else
		{
			Err();
		}


	}
}
void ShowUsers(int mode)
{
	if (mode == 0)
	{

		system("cls");
		std::cout << "№\t" << std::left << std::setw(12) << "Логин\t\t" << "Пароль\t\t\t" << "Роль\n";
		for (size_t i = 1; i < userSize; i++)
		{
			std::cout << i << "\t" << std::left << std::setw(8) << loginArr[i] << "\t\t" << passArr[i]
				<< "\t\t\t" << statusArr[i] << "\n";
		}
		Sleep(2000);
	}
	else if (mode == 1)
	{
		std::cout << "№\t" << std::left << std::setw(12) << "Логин\t\t" << "Пароль\t\t\t" << "Роль\n";
		for (size_t i = 0; i < userSize; i++)
		{
			std::cout << i << "\t" << std::left << std::setw(8) << loginArr[i] << "\t\t" << passArr[i]
				<< "\t\t\t" << statusArr[i] << "\n";
		}

	}
}
bool Logout();
void AddNewItem()
{
	std::string newName, newPrice, newCount, choose;
	double price = 0.0, count = 0.0;
	bool exit = true;
	while (true)
	{


		while (true)
		{
			system("cls");

			std::cout << "\tДобавление нового товара!\n\nВведите \"exit\" для отмены добавления\n\n";

			std::cout << "Введите название нового товара: ";
			Getline(newName);

			if (newName == "exit")
			{
				std::cout << "Отмена добавления товара!\n\n";
				Sleep(1500);
				break;
			}
			if (newName.size() <= 0 || newName.size() >= 50)
			{
				std::cout << "Максимальная длина названия 50 символов\n";
				Sleep(1500);

			}
			else
			{
				break;
			}
		}
		while (exit)
		{
			std::cout << "Введите кол-во нового товара: ";
			Getline(newCount);

			if (newCount == "exit")
			{
				std::cout << "Отмена добавления товара!\n\n";
				Sleep(1500);
				exit = false;
				break;
			}
			if (IsNumber(newCount))
			{
				count = std::stoi(newCount);
				if (count > maxItemSize)
				{
					std::cout << "Превышен максимальный размер товара. До " << maxItemSize << " шт.\n\n";

				}
				else
				{
					break;
				}

			}

		}
		while (exit)
		{
			system("cls");
			std::cout << "\tДобавление нового товара!\n\nВведите \"exit\" для отмены добавления\n\n";
			std::cout << "Введите цену нового товара: ";
			Getline(newPrice);

			if (newPrice == "exit")
			{
				std::cout << "Отмена добавления товара!\n\n";
				Sleep(1500);
				exit = false;
				break;
			}
			if (IsNumber(newPrice))
			{
				price = std::stod(newPrice);
				if (price > 10000000)
				{
					std::cout << "Превышена максимальная цена. До " << 10000000 << " руб.\n\n";
					Sleep(1500);
				}
				else
				{
					break;
				}

			}

		}
		if (exit)
		{
			std::cout << "Название товара: " << newName << "\nКол-во: " << count << "\nЦена: " << price << "/n/n";
			std::cout << "Подтвердить?\n1 - Да\n2 - Нет\nВыбор: ";
			Getline(choose);

			if (choose == "1")
			{
				storageSize++;
				unsigned int* idArrTemp = new unsigned int[storageSize];
				std::string* nameArrTemp = new std::string[storageSize];
				unsigned int* countArrTemp = new unsigned int[storageSize];
				double* priceArrTemp = new double[storageSize];
				FillArr(idArrTemp, idArr, storageSize - 1);
				FillArr(nameArrTemp, nameArr, storageSize - 1);
				FillArr(countArrTemp, coutArr, storageSize - 1);
				FillArr(priceArrTemp, priceArr, storageSize - 1);

				idArrTemp[storageSize - 1] = storageSize;
				nameArrTemp[storageSize - 1] = newPrice;
				priceArrTemp[storageSize - 1] = price;
				countArrTemp[storageSize - 1] = count;

				std::swap(idArr, idArrTemp);
				std::swap(nameArr, nameArrTemp);
				std::swap(coutArr, countArrTemp);
				std::swap(priceArr, priceArrTemp);

				delete[]idArrTemp, nameArrTemp, countArrTemp, priceArrTemp;
				std::cout << "Загрузка...";
				Sleep(2000);
				std::cout << "Товар успешно добавлен!\n\n";
				Sleep(1500);
			}
			else if (choose == "2")
			{
				std::cout << "Отмена\n\n";
				Sleep(1500);
			}
			else
			{
				Err();
			}


		}
		if (exit == false)
		{
			break;
		}
	}

	system("pause");
	//storageSize++;
	//unsigned int* idArrTemp = new unsigned int[storageSize];
}
void DeleteItem()
{
	std::string chooseId, choose;
	unsigned int id = 0;

	while (true)
	{
		system("cls");
		ShowStorage(3);
		std::cout << "\nВведите id товара для удаления или \"exit\" для отмены: ";
		Getline(chooseId);
		if (chooseId == "exit")
		{
			std::cout << "Отмена удаления товара!\n\n";
			Sleep(1500);
			break;
		}
		if (IsNumber(chooseId))
		{
			id = std::stoi(chooseId) - 1;
			if (id < 0 || id > storageSize - 1)
			{
				std::cout << "Неверный ID!\n";
				Sleep(1500);
			}
			else
			{
				std::cout << "Удаляемый товар: " << nameArr[id] << "\n\n";
				std::cout << "Подтвердить?\n1 - Да\n2 - Нет\nВыбор: ";
				Getline(choose);
				if (choose == "1")
				{
					storageSize--;
					unsigned int* idArrTemp = new unsigned int[storageSize];
					std::string* nameArrTemp = new std::string[storageSize];
					unsigned int* countArrTemp = new unsigned int[storageSize];
					double* priceArrTemp = new double[storageSize];
					for (size_t i = 0, c = 0; i < storageSize; i++, c++)
					{
						idArrTemp[i] = i + 1;
						nameArrTemp[i] = nameArr[c];
						countArrTemp[i] = coutArr[c];
						priceArrTemp[i] = priceArr[c];
					}
					std::swap(idArr, idArrTemp);
					std::swap(nameArr, nameArrTemp);
					std::swap(coutArr, countArrTemp);
					std::swap(priceArr, priceArrTemp);

					delete[]idArrTemp, nameArrTemp, countArrTemp, priceArrTemp;
					std::cout << "Загрузка...";
					Sleep(2000);
					std::cout << "Товар успешно удален!\n\n";
					Sleep(1500);
					break;
				}
			}
		}
	}


}
void ChandeName()
{
	std::string chooseId, newName, choose;
	unsigned int id = 0;
	while (true)
	{
		system("cls");
		ShowStorage(3);
		std::cout << "Введите id товара или \"exit\"для отмены: ";
		Getline(chooseId);

		if (chooseId == "exit")
		{
			std::cout << "Отмена изменения названия!\n\n";
			Sleep(1500);
			break;
		}

		std::cout << "\tВведите название нового товара: ";

		Getline(newName);


		if (newName.size() <= 0 || newName.size() >= 50 || newName == "exit")
		{
			std::cout << "Максимальная длина названия 50 символов\n";
			Sleep(1500);

		}
		else if (IsNumber(chooseId))
		{
			id = std::stoi(chooseId) - 1;
			if (id < 0 || id > storageSize - 1)
			{
				std::cout << "Неверный ID!\n";
				Sleep(1500);
			}
			else
			{
				std::cout << "\n" << std::left << std::setw(25)
					<< nameArr[id] << " ---> " << newName << "\n\n";
				std::cout << "Подтвердить?\n1 - Да\n2 - Нет\nВыбор: ";
				Getline(choose);
				if (choose == "1")
				{
					nameArr[id] = newName;
					std::cout << "Название товара успешно изменено\n";
					Sleep(1500);
					break;
				}
				else if (choose == "2")
				{
					std::cout << "Отмена\n";
					Sleep(1500);
				}
				else
				{
					Err();
				}
			}

		}
	}
}
void DeleteUser()
{
	std::string chooseNumber, choose, checkPass;
	int userNumber = 0;
	int isAdmin = 0;
	while (true)
	{

		if (currentStatus == userStatus[0])
		{
			if (userSize < 2)
			{
				std::cout << "Недостаточно пользователей для удаления\n";
				Sleep(1500);
				return;
			}
			ShowUsers();
			isAdmin = 1;
		}
		else if (currentStatus == userStatus[1])
		{
			if (staffCount < 1)
			{
				std::cout << "Недостаточно сотрудников для удаления\n";
				Sleep(1500);
				return;
			}
		}


		ShowUsers();
		isAdmin = 1;

		std::cout << "\nВведите номер пользователя для удаления или \"exit\" для отмены: ";
		Getline(choose);

		if (choose == "exit")
		{
			std::cout << "Отмена удаления пользователя!\n";
			Sleep(1500);
			break;
		}


		else if (IsNumber(choose))
		{
			userNumber = std::stoi(choose);

			if (userNumber < isAdmin || userNumber > userSize - 1)
			{
				std::cout << "Пользователь с таким номером не существует!\n";
				Sleep(1500);

			}
			for (size_t i = isAdmin; i < userSize; i++)
			{
				if (i == userNumber)
				{
					system("cls");

					if (currentStatus == userStatus[1] && statusArr[userNumber] != userStatus[2])
					{
						std::cout << "Нельзя удалить администратора\n";
						Sleep(1500);
						break;
					}

					std::cout << "Удаляемый пользователь: " << loginArr[i] << "\n\n";
					std::cout << "Для подтверждения введите пароль или \"exit\" для отмены: ";
					Getline(choose);

					if (choose == "exit")
					{
						std::cout << "Отмена удаления пользователя!\n";
						Sleep(1500);
						break;
					}
					else if (checkPass == passArr[0])
					{
						userSize--;
						if (statusArr[userNumber] == userStatus[2])
						{
							staffCount--;
						}
						std::string* loginArrTemp = new std::string[userSize];
						std::string* passArrTemp = new std::string[userSize];
						std::string* statusArrTemp = new std::string[userSize];
						double* salesArrTemp = new double[userSize];
						unsigned int* userIdArrTemp = new unsigned int[userSize];
						for (size_t i = 0, c = 0; i < userSize; i++, c++)
						{
							if (userNumber == c)
							{
								c++;
							}
							loginArrTemp[i] = loginArr[c];
							passArrTemp[i] = passArr[c];
							statusArrTemp[i] = statusArr[c];
							salesArrTemp[i] = salesArr[c];
							userIdArrTemp[i] = userIdArr[c];

						}
						std::swap(loginArrTemp, loginArr);
						std::swap(passArrTemp, passArr);
						std::swap(statusArrTemp, statusArr);
						std::swap(salesArrTemp, salesArr);
						std::swap(userIdArrTemp, userIdArr);

						delete[] loginArrTemp, passArrTemp, statusArrTemp;
						std::cout << "Загрузка...";
						Sleep(2000);
						std::cout << "Пользователь успешно удален!\n\n";
						Sleep(1500);
						break;
					}
					else
					{
						std::cout << "Неверный пароль\n";
						Sleep(1500);
						i--;
					}

				}
			}

		}
	}
}
void StorageReturner()
{
	for (size_t i = 0; i < checkSize; i++)
	{
		coutArr[idArrCheck[i] - 1] += countArrCheck[i];
	}
	delete[] idArrCheck;
	delete[] nameArrCheck;
	delete[] countArrCheck;
	delete[] priceArrCheck;
	delete[] totalPriceArrCheck;
	idArrCheck;
	nameArrCheck = nullptr;
	countArrCheck = nullptr;
	priceArrCheck = nullptr;
	totalPriceArrCheck = nullptr;
	checkSize = 0;
}
void ShowIncome()
{
	system("cls");
	std::cout << "Статистика доходов за день\n\n";
	std::cout << "Наличные поступления: " << "\n";
	std::cout << "Безналичные поступления: " << bankIncome << "\n";
	std::cout << "Итого: " << bankIncome + cashIncome << "\n\n";
	std::cout << "Ваша личная продажа: " << salesArr[currentId] << "\n\n";

	system("pause");
	system("cls");
}
void ChangePass()
{
	std::string newPass1, newPass2, choose;
	int userNumber = 0;
	int isAdmin = 0;
	while (true)
	{
		if (currentStatus == userStatus[0])
		{
			ShowUsers(1);
			isAdmin = 0;
		}
		else
		{
			ShowUsers();
			isAdmin = 1;
		}

		std::cout << "\nВведите пользователя или \"exit\" для отмены: ";
		Getline(choose);

		if (choose == "exit")
		{
			std::cout << "Отмена изменения пароля!\n";
			Sleep(1500);
			break;
		}
		if (IsNumber(choose))
		{
			userNumber = std::stoi(choose);


			if (userNumber < isAdmin || userNumber > userSize - 1)
			{
				std::cout << "Пользователь с таким номером не существует!\n";
				Sleep(1500);
				break;
			}

			if (currentStatus == userStatus[1] && statusArr[userNumber] == userStatus[1])
			{
				std::cout << "Нельзя изменить пароль администратору\n";
				Sleep(1500);
				break;
			}
			for (size_t i = 0; i < userSize; i++)
			{
				if (i == userNumber)
				{
					system("cls");
					std::cout << "Введите новый пароль для пользователя " << loginArr[i] << ": ";
					Getline(newPass1);
					std::cout << "Повторите новый пароль для пользователя " << loginArr[i] << ": ";
					Getline(newPass2);
					if (newPass1 == newPass2)
					{
						passArr[i] = newPass1;
						std::cout << "Успешно\n";
						Sleep(1500);
						break;
					}
					else
					{
						std::cout << "Пароли не совпадают\n";
						Sleep(1500);
						i--;
					}
				}
			}


		}
	}
}
void CreateStorage()
{
	const size_t staticSize = 10;
	unsigned int id[staticSize]{ 1,2,3,4,5,6,7,8,9,10 };
	std::string name[staticSize]
	{
		"Малиновый рулет", "Кукис", "Пиво", "Какао", "Кофе", "Морс чёрная смородина",
"Чизкейк Нью-Йорк", "Шоколадный тарт", "Маффин с чёрной смородиной",
"Зелёный чай с лесными ягодами"

	};
	double price[staticSize]{ 320, 120, 100, 350, 220, 55, 250, 200, 60, 40 };
	unsigned int count[staticSize]{ 20, 10, 15, 3, 2, 50, 25, 8, 60, 40 };

	if (isStorageCreated)
	{
		delete[]idArr, nameArr, coutArr, priceArr;
	}

	storageSize = staticSize;
	idArr = new unsigned int[storageSize];
	nameArr = new std::string[storageSize];
	coutArr = new unsigned int[storageSize];
	priceArr = new double[storageSize];
	isStorageCreated = true;

	FillArr(idArr, id, storageSize);
	FillArr(nameArr, name, storageSize);
	FillArr(coutArr, count, storageSize);
	FillArr(priceArr, price, storageSize);
}

void CreateNewStorage()
{
		std::string* storageName = new std::string;
		int* itemCount = new int;

		std::cout << "\n\n\n===Создайте новый склад===\n\n\n";

		bool name = false;
		while (name == false)
		{
			std::cout << "Введите название склада: ";
			char nameTemp[100];
			std::cin >> nameTemp;
			*storageName = nameTemp;

			std::string checkName = *storageName;

			if (checkName.length() > 0)
			{
				name = true;
			}
			else
			{
				std::cout << "Ошибка!! Название не может быть пустым!\n";
				continue;
			}
		}

		bool count = false;
		while (count == false)
		{
			std::cout << "Сколько разных товаров будет храниться на складе? ";
			std::cin >> *itemCount;

			if (*itemCount > 0 && *itemCount <= 10)
			{
				count = true;
			}
			else
			{
				std::cout << "Ошибка!! Введите число от 1 до 10\n";
				continue;
			}
		}

		std::string* itemNames = new std::string[*itemCount];
		int* itemQuantities = new int[*itemCount];
		double* itemPrices = new double[*itemCount];

		int i = 0;
		while (i < *itemCount)
		{
			std::cout << "\nТовар " << (i + 1) << " из " << *itemCount << ":\n";

			bool itemNameOk = false;
			while (itemNameOk == false)
			{
				std::cout << "Название товара: ";
				char itemBuffer[100];
				std::cin >> itemBuffer;
				itemNames[i] = itemBuffer;

				if (itemNames[i].size() > 0)
				{
					itemNameOk = true;
				}
				else
				{
					std::cout << "Название товара не может быть пустым!\n";
					continue;
				}
			}

			bool quantityOk = false;
			while (quantityOk == false)
			{
				std::cout << "Количество: ";
				std::cin >> itemQuantities[i];

				if (itemQuantities[i] >= 0)
				{
					quantityOk = true;
				}
				else
				{
					std::cout << "Ошибка!! Количество не может быть отрицательным!\n";
					continue;
				}
			}

			bool priceOk = false;
			while (priceOk == false)
			{
				std::cout << "Цена: ";
				std::cin >> itemPrices[i];

				if (itemPrices[i] >= 0)
				{
					priceOk = true;
				}
				else
				{
					std::cout << "Ошибка!! Цена не может быть отрицательной!\n";
					continue;
				}
			}

			i = i + 1;
		}

		std::cout << "\n\n\n===Новый склад создан===\n\n\n";
		std::cout << "Название склада: " << *storageName << "\n";
		std::cout << "Количество разных товаров: " << *itemCount << "\n";
		std::cout << "\nСписок товаров:\n";

		int j = 0;
		while (j < *itemCount)
		{
			std::cout << j + 1 << ". ";
			std::string displayItemName = itemNames[j];
			std::cout << displayItemName;
			std::cout << " - " << itemQuantities[j] << " шт.\n";
			std::cout << " - " << itemPrices[j] << " руб.\n";
			j = j + 1;
		}

		if (isStorageCreated)
		{
			delete[] idArr;
			delete[] nameArr;
			delete[] coutArr;
			delete[] priceArr;
		}

		storageSize = *itemCount;
		idArr = new unsigned int[storageSize];
		nameArr = new std::string[storageSize];
		coutArr = new unsigned int[storageSize];
		priceArr = new double[storageSize];
		isStorageCreated = true;

		for (int a = 0; a < storageSize; a++)
		{
			idArr[a] = a + 1;
			nameArr[a] = itemNames[a];
			coutArr[a] = itemQuantities[a];
			priceArr[a] = itemPrices[a];
		}

		delete[] itemNames;
		delete[] itemQuantities;
		delete[] itemPrices;
		delete storageName;
		delete itemCount;

		system("pause");
		system("cls");
	}

bool CheckLogin(const std::string& str)
{
	if (str.size() < 5 || str.size() >= 20)
	{
		std::cout << "Некорректная длина логина от 5 до 20\n";
		Sleep(1500);
		return false;
	}

	for (char sym : str)
	{
		if (!specialSymbols.count(sym))
		{
			std::cout << "Недопустимый символ в логине!\n\n";
			Sleep(1500);
			return false;
		}
	}
	for (size_t i = 0; i < userSize; i++)
	{
		if (str == loginArr[i])
		{
			std::cout << "Логин уже занят\n\n";
			Sleep(1500);
			return false;
		}
	}
}
void ShowStorage(int mode)
{
	if (mode == 0)
	{
		std::cout << "ID\t" << std::left << std::setw(25) << "Название товара\t\t"
			<< "Цена\t" << "Кол-во\n";
		for (size_t i = 0; i < storageSize; i++)
		{
			std::cout << idArr[i] << "\t" << std::left << std::setw(25) << nameArr[i] << "\t"
				<< priceArr[i] << "\t" << coutArr[i] << "\n";
		}
		system("pause");
		system("cls");
	}
	else if (mode == 1)
	{
		std::cout << "ID\t" << std::left << std::setw(25) << "Название товара\t" << "\n"
			<< "Кол-во\n";
		for (size_t i = 0; i < storageSize; i++)
		{
			std::cout << idArr[i] << "\t" << std::left << std::setw(25) << nameArr[i] << "\t"
				<< "\t" << coutArr[i] << "\n";
		}

	}
	else if (mode == 2)
	{
		std::cout << "ID\t" << std::left << std::setw(25) << "Название товара\t\t"
			<< "Цена\t\n";
		for (size_t i = 0; i < storageSize; i++)
		{
			std::cout << idArr[i] << "\t" << std::left << std::setw(25) << nameArr[i] << "\t"
				<< priceArr[i] << "\t" << "\n";
		}
		system("pause");
		system("cls");

	}
	else if (mode == 3)
	{
		std::cout << "ID\t" << std::left << std::setw(25) << "Название товара\t\t"
			<< "Цена\t\n";
		for (size_t i = 0; i < storageSize; i++)
		{
			std::cout << idArr[i] << "\t" << std::left << std::setw(25) << nameArr[i] << "\n";

		}

	}
}
void ChangeAccount()
{
	std::string choose;
	if (isSetCreated = false)
	{
		SetSpecialSymbols();
	}
	if (isPassSetCreated == false)
	{
		SetPassSymbols();
	}
	while (true)
	{
		system("cls");
		std::cout << "1 - Добавить нового пользователя\n";
		std::cout << "2 - Просмотреть пользователей\n";
		std::cout << "3 - Изменить пароль пользователя\n";
		std::cout << "4 - Удалить пользователя\n";
		std::cout << "0 - Выйти в главное меню\n";
		std::cout << "Выбор: ";
		Getline(choose);
		if (choose == "1" && storageSize > 1)
		{
			AddNewUser();
		}
		else if (choose == "2" && storageSize > 1)
		{
			ShowUsers();
		}
		else if (choose == "3" && storageSize > 0)
		{
			ChangePass();
		}
		else if (choose == "4" && storageSize > 0)
		{
			DeleteUser();
		}
		else if (choose == "0")
		{
			DeleteUser();
		}
		else
		{
			Err();
		}


	}
}
void AddStorageItem()
{
	std::string chooseId, chooseCount, choose;
	unsigned int id = 0, count = 0;
	while (true)
	{
		system("cls");
		ShowStorage(1);
		std::cout << "Введите ID товара или \"exit\" для выхода";
		Getline(chooseId);
		if (chooseId == "exit")
		{

			std::cout << "Отмена добавления товара!\n";
			Sleep(1500);
			system("cls");
			break;

		}
		std::cout << "Введите кол-во товара для добавления";
		Getline(chooseCount);

		if (IsNumber(chooseId) && IsNumber(chooseCount))
		{
			id = std::stoi(chooseId) - 1;
			count = std::stoi(chooseCount);

			if (id < 0 || id > storageSize - 1 || count < 0 || count > 299)
			{
				std::cout << "Некорректный id или кол-во\nМаксимальное кол-во - " << maxItemSize << "\n\n";
				Sleep(1500);

			}
			else
			{
				std::cout << std::left << std::setw(25) << nameArr[id] << "\t"
					<< coutArr[id] << " ---> " << coutArr[id] + count << "\n\n";
				std::cout << "Подтвердить?\n1 - Да\n2 - Нет\nВыбор: ";
				Getline(choose);
				if (choose == "1")
				{
					coutArr[id] += count;
					std::cout << "Товар успешно добавлен\n\n";
					Sleep(1500);
					system("cls");
					break;


				}
				else if (choose == "2")
				{
					std::cout << "Отмена добавления!\n";
					Sleep(1500);
					system("cls");
				}
				else
				{
					Err();
				}
			}


		}


	}

}
void RemoveStorageItem()
{


	std::string chooseId, chooseCount, choose;
	unsigned int id = 0, count = 0;
	while (true)
	{
		system("cls");
		ShowStorage(1);
		std::cout << "Введите ID товара или \"exit\" для выхода";
		Getline(chooseId);
		if (chooseId == "exit")
		{

			std::cout << "Отмена удаления товара!\n";
			Sleep(1500);
			system("cls");
			break;

		}
		std::cout << "Введите кол-во товара для удаления";
		Getline(chooseCount);

		if (IsNumber(chooseId) && IsNumber(chooseCount))
		{
			id = std::stoi(chooseId) - 1;
			count = std::stoi(chooseCount);

			if (id < 0 || id > storageSize - 1 || count < 0 || count > coutArr[id])
			{
				std::cout << "Некорректный id или кол-во\nМаксимальное кол-во - " << coutArr[id] << "\n\n";
				Sleep(1500);

			}
			else
			{
				std::cout << std::left << std::setw(25) << nameArr[id] << "\t"
					<< coutArr[id] << " ---> " << coutArr[id] + count << "\n\n";
				std::cout << "Подтвердить?\n1 - Да\n2 - Нет\nВыбор: ";
				Getline(choose);
				if (choose == "1")
				{
					coutArr[id] += count;
					std::cout << "Товар успешно удален\n\n";
					Sleep(1500);
					system("cls");
					break;


				}
				else if (choose == "2")
				{
					std::cout << "Отмена удаления!\n";
					Sleep(1500);
					system("cls");
				}
				else
				{
					Err();
				}
			}


		}


	}


}
void ShowSuperAdminMenu()
{
	std::string choose;
	while (true)
	{
		std::cout << "1 - Начать продажу\n";
		std::cout << "2 - Просмотреть склад\n";
		std::cout << "3 - Добавить товар\n";
		std::cout << "4 - Удалить товар\n";
		std::cout << "5 - Изменить цену\n";
		std::cout << "6 - Редактировать склад\n";
		std::cout << "7 - Редактировать пользователей\n";
		std::cout << "8 - Доходы за день\n";
		std::cout << "0 - Выйти из аккаунта\n";
		std::cout << "Выбор: ";
		Getline(choose);
		if (choose == "1" && storageSize > 0)
		{

			Selling();

		}
		else if (choose == "2" && storageSize > 0)
		{
			ShowStorage();
		}
		else if (choose == "3" && storageSize > 0)
		{
			AddStorageItem();
		}
		else if (choose == "4" && storageSize > 0)
		{
			RemoveStorageItem();
		}
		else if (choose == "5" && storageSize > 0)
		{
			ChangePrice();
		}

		else if (choose == "6")
		{
			ChangeStorage();
		}

		else if (choose == "7")
		{
			ChangeAccount();
		}

		else if (choose == "8")
		{
			ShowIncome();
		}

		else if (choose == "0")
		{
			if (Logout() == true)
			{
				break;
			}

		}
		else
		{
			Err();
		}

	}


}
void ShowAdminMenu()
{
	std::string choose;
	while (true)
	{
		std::cout << "1 - Начать продажу\n";
		std::cout << "2 - Просмотреть склад\n";
		std::cout << "3 - Добавить товар\n";
		std::cout << "4 - Удалить товар\n";
		std::cout << "5 - Изменить цену\n";
		std::cout << "6 - Редактировать склад\n";
		std::cout << "7 - Редактировать пользователей\n";
		std::cout << "8 - Доходы за день\n";
		std::cout << "0 - Выйти из аккаунта\n";
		std::cout << "Выбор: ";
		Getline(choose);
		if (choose == "1" && storageSize > 0)
		{

			Selling();

		}
		else if (choose == "2" && storageSize > 0)
		{
			ShowStorage();
		}
		else if (choose == "3" && storageSize > 0)
		{
			AddStorageItem();
		}
		else if (choose == "4" && storageSize > 0)
		{
			RemoveStorageItem();
		}


		else if (choose == "5")
		{
			ChangeStorage();
		}

		else if (choose == "6")
		{
			ChangeAccount();
		}

		else if (choose == "7")
		{
			ChangeAccount();
		}

		else if (choose == "0")
		{
			break;
		}
		else
		{
			Err();
		}

	}
}
void ShowuserMenu()
{
	std::string choose;
	while (true)
	{
		std::cout << "1 - Начать продажу\n";
		std::cout << "2 - Просмотреть склад\n";
		std::cout << "8 - Доходы за день\n";
		std::cout << "0 - Выйти из аккаунта\n";
		std::cout << "Выбор: ";
		Getline(choose);
		if (choose == "1" && storageSize > 0)
		{

			Selling();

		}
		else if (choose == "2" && storageSize > 0)
		{
			ShowStorage();
		}

		else if (choose == "3")
		{
			ShowIncome();
		}

		else if (choose == "0")
		{
			if (Logout() == true)
			{
				break;
			}

		}
		else
		{
			Err();
		}

	}


}
bool IsNumber(const std::string& str)
{
	if (str.size() <= 0 || str.size() >= 10)
	{
		std::cout << "Некорректный ввод\n";
		std::cout << "Можно вводить числа. От 1 до 9 символов допустимо\n\n";
		Sleep(1500);
		return false;

	}
	for (size_t i = 0; i < str.size(); i++)
	{
		if (!std::isdigit(str[i]))
		{
			std::cout << "Некорректный ввод\n";
			std::cout << "Можно вводить только цифры\n\n";
			Sleep(1500);
			return false;

		}
	}
	return true;
}
void Start()
{
	std::string choose;
	std::cout << "\n\n\n===Семёрочка===\n\n\n";
	while (true)
	{
		if (Login())
		{
			system("cls");
			if (currentStatus == userStatus[0])
			{
				while (true)
				{
					std::cout << "Выберите тип склада\n1 - Готовый\n2 - Новый\nВыбор: ";
					Getline(choose);
					if (choose == "1")
					{
						if (isStorageCreated == false)
						{
							CreateStorage();
						}

						system("cls");
						ShowSuperAdminMenu();
						break;
					}
					else if (choose == "2")
					{
						if (isStorageCreated == false)
						{
							CreateNewStorage();
						}
						ShowSuperAdminMenu();
						break;
					}
					else
					{
						Err();
					}
				}

			}
			else if (currentStatus == userStatus[1])
			{
				if (isStorageCreated == false)
				{
					CreateStorage();
				}
				ShowAdminMenu();
			}
			else if (currentStatus == userStatus[2])
			{
				if (isStorageCreated == false)
				{
					CreateStorage();
				}
				system("cls");
				ShowAdminMenu();
				break;
			}

		}
		else if (currentStatus == userStatus[0] && currentStatus == userStatus[1])
		{
			system("cls");
			std::cout << "Общий доход за день: " << cashIncome + bankIncome;
			std::cout << "\n\n\tПрограмма завершена\n";
			Sleep(2000);
			system("cls");
			break;
		}
	}
}
bool Login()
{
	std::string login, pass;
	while (true)
	{
		std::cout << "Введите логин: ";
		Getline(login);
		std::cout << "Введите пароль: ";
		Getline(pass);

		if (login == "exit" && pass == "exit")
		{
			currentStatus = "";
			return false;


		}
		if (login == loginArr[0] && pass == passArr[0])
		{
			std::cout << "Пользователь: " << loginArr[0] << "\n\nВход выполнен!\n\n";
			std::cout << "Ваш статус: " << userStatus[0] << "\n\n";
			currentStatus = statusArr[0];
			currentId = userIdArr[0];
			return true;
		}
		for (size_t i = 1; i < userSize; i++)
		{
			if (login == loginArr[i] && pass == passArr[i])
			{
				std::cout << "Пользователь: " << loginArr[0] << "\n\nВход выполнен!\n\n";
				std::cout << "Ваш статус: Пользователь\n\n";
				currentStatus = statusArr[i];
				currentId = userIdArr[i];
				return true;
			}

		}
		Err();
		Sleep(1500);
		system("cls");

	}
}
inline void Getline(std::string& str)
{
	std::getline(std::cin, str, '\n');
}

inline void Err()
{
	std::cout << "Некорректный ввод\n\n";
}

bool Logout()
{
	std::string choose;
	while (true)
	{
		system("cls");
		std::cout << "Вы действительно хотите выйти из аккаунта? Введите пароль или \"exit\" для отмены.";
		Getline(choose);
		if (choose == "exit")
		{
			system("cls");
			return false;
		}
		else if (choose == passArr[currentId - 1] || choose == passArr[0])
		{
			system("cls");
			return true;
		}
		else
		{
			Err();
		}
	}
}

template<typename ArrType>
void FillArr(ArrType* dynamicArr, ArrType* staticArr, size_t arraySize)
{
	for (size_t i = 0; i < arraySize; i++)
	{
		dynamicArr[i] = staticArr[i];
	}
}
