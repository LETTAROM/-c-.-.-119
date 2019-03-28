
//Потоковый ввод вывод в файл c++.Перегрузка операторов.Урок #119
#include<string>
#include<iostream>
#include<fstream>
using namespace std;
class Point
{
public:
	Point()
	{
		x = y = z = 0;
	}
	Point(int x, int y, int z)
	{
		this->x = x;
		this->y = y;
		this->z = z;
	}
// если тут private: то поля в перегр-х операторах  б недоступны, тогда н. делать либо геттереы и сеттеры
	//либо делать операторы дружественными
	int x;
	int y;
	int z;
};

ostream& operator<<(ostream& os,const Point& point)//этот оператор позв-т записывать данные в файл
//ostream&  - тип возвр знач.Этот метод возвр-т ссылку на объект
//типа ostream.Тк все влассы типа ostream,fstream,ifstreamсвязаны цепочкой наследованием
//то данный оператор м. использ-ь и для др таких классовтакже cout //cout << p;
{//здесь пишем, как объект б. выводиться в ф. и работат с консолью
//пусть наш объект ю выводиться в cout  с полями
	os << point.x << " " << point.y << " " << point.z << " " << endl;
	return os;
}
istream& operator>>(istream& is,  Point& point)//этот оператор вытаскивает данные из ф
{//тк в объект point мы б. менять, б в него считывать данные, то он не должен б. конст
	is >> point.x >>point.y >>point.z ;
	   	return is;
}
int main()
{	setlocale(LC_ALL, "rus");
	string path = "myfile.txt";
	//Point p(121, 245, 241);//данный м. менять , они б добавляться в ф
	//cout << p;//рез-т 121, 245, 241, следует из метода ostream& operator
			
	fstream fs;
	fs.open(path, fstream::in | fstream::out | fstream::app);
	if (!fs.is_open())
	{
		cout << "Файл не открывается. Ошибка" << endl;
	}
	else
	{
		cout << "Файл открыт!" << endl;
		/*fs << p  << "\n";*///запишет данные - рез-т 121, 245, 241 в наш файлmyfile
		while (true)//м так while (!fs.eof()) пока ф. не конец. б выводить данные на консоль б. создавать перем p 
	{		//туда сохранять данные и выводить их н аконсоль с помощью оператора istream& operator>>(istream& is,  Point& point)
	Point p;
fs >> p;//теперь м. получать данные с помощью istream& operator>>(istream& is,  Point& point)
if (fs.eof())//чт. не выодились нули из объекта Point. Мы делаем еще проверку на конец файла, т е если
//ф. закончен то м ывыходим из него
{
	break;
}
		cout << p << endl;
		}
	}
	fs.close();
	return 0;
}
