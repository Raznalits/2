![image](https://user-images.githubusercontent.com/91465477/150936501-a6839f34-f5fb-41f4-9908-7e901c05fa43.png)



using namespace cv;
using namespace std;

void my_mouse_callback(int event, int x, int y, int flags, void* param);
void reading(string Filename, string imagename, double wl, double hl, double wr, double hr);
void file();

int main(int argc, char** argv) {
	
	setlocale(LC_ALL, "Russian");
	

	const char* imagename = "home.png";
	const char* traectory_file = "coordinat.txt";
	double wl = 55.639157;
	double hl = 37.832042;
	double wr = 55.638865;
	double hr = 37.832663;

	string check;
	cout << "read or enter" << endl;
	cin >> check;
	if (check == "read") {
		reading("coordinat.txt", "home.png", wl, hl, wr, hr);
	}
	else if (check == "enter") {
		Mat image = imread("home.png");
		namedWindow("modernGoogle");
		setMouseCallback("modernGoogle", my_mouse_callback, &image);
		while (true) {
			imshow("modernGoogle", image);
			waitKey(30);
		}
	}
	else {
		cout << "Ошибка ввода" << endl;
		waitKey(30);
	}
	return(0);
}

void file()
{
	ofstream file;
	file.open("test.txt", ios_base::app);
	unsigned long milliseconds_since_epoch = chrono::system_clock::now().time_since_epoch() / chrono::milliseconds(1);
	file << milliseconds_since_epoch << endl;
	file.close();
	Mat image = imread("home.png");
	namedWindow("modernGoogle");
	setMouseCallback("modernGoogle", my_mouse_callback, &image);
	while (true) {
		imshow("modernGoogle", image);
		waitKey(30);
	}
}

void my_mouse_callback(int event, int x, int y, int flags, void* param)
{
	if (event == EVENT_LBUTTONDOWN)
	{
		Mat* pImage = (Mat*)param;
		Mat image = *pImage;
		Point trackBox;
		trackBox = Point(x, y);

		Mat small = imread("da.jpg");
		Mat im;
		im = image(Rect(x, y, small.cols, small.rows));

		addWeighted(small, 0.6, im, 0.4, 0, im);


		ofstream file;
		file.open("coordinat.txt", ios_base::app);
		unsigned long milliseconds_since_epoch = chrono::system_clock::now().time_since_epoch() / chrono::milliseconds(1);
		file << milliseconds_since_epoch << " " << x << " " << y << endl;
		file.close();
	}
}

void reading(string Filename, string imagename, double wl, double hl, double wr, double hr)
{
	string File = Filename;
	int x, y, xm, ym;
	unsigned long t, tt;
	long double shirota, dolgota;

	ifstream f(File);
	if (!(f.is_open()))  cout << "Ошибка" << File;
	else
	{
		Mat img = imread(imagename);
		cout << "Размер изображения: " << endl;
		cout << "Ширина: " << img.size().width << endl;
		cout << "Высота: " << img.size().height << endl;
		//вычисляем коэффициент масштабирования
		long double koef_mashtabW, koef_mashtabH;
		koef_mashtabW = (wl - wr) / img.size().width;
		koef_mashtabH = (hr - hl) / img.size().height;
		cout << koef_mashtabW << koef_mashtabH << endl;

		f >> t >> x >> y;

		cout << "Ваш путь пролегает по следующему  маршруту: " << endl;
		while (!f.eof())
		{
			f >> tt >> xm >> ym;

			shirota = koef_mashtabW * (img.size().width - x) + wr;
			dolgota = koef_mashtabH * (img.size().height - y) + hl;
			cout << "Метка времени: " << t << " широта: " << shirota << " долгота: " << dolgota << endl;

			Mat small = imread("da.jpg");
			Mat im;
			im = img(Rect(x, y, small.cols, small.rows));

			addWeighted(small, 0.6, im, 0.4, 0, im);

			x = xm;
			y = ym;
			imshow("route", img);
			waitKey(tt - t);
			t = tt;
		}
	}
	f.close();
	cout << endl;
}


