# 2
1. Наложение изображений: addWeighted(small, 0.6, im, 0.4, 0, im);
2. Как происходит чтение из файла: ifstream >>
3. Как определить координаты щелчка мыши: int x, int y
4-5. Как определить и вывести разницу во времени между щелчками мыши: 
cout << "Ваш путь пролегает по следующему  маршруту: " << endl;
	while (!f.eof())
	{
		f >> tt;
		cout << "Метка времени: " << t << endl;
		imshow("route", img);
		cont << (tt - t);
		t = tt;
  }
