# codeAndDocs

#include <iostream>
#include <string>

std::string edinici[10] = { "", "один", "два", "три", "четыре", "пять", "шесть", "семь", "восемь", "девять" };
std::string edinici_zh[10] = { "", "одна", "две", "три", "четыре", "пять", "шесть", "семь", "восемь", "девять" };
std::string desyatki1[10] = { "десять", "одиннадцать", "двенадцать", "тринадцать", "четырнадцать", "пятнадцать", "шестнадцать", "семнадцать", "восемнадцать", "девятнадцать" };
std::string desyatki2[10] = { "", "", "двадцать", "тридцать", "сорок", "пятьдесят", "шестьдесят", "семьдесят", "восемьдесят", "девяносто" };
std::string sotni[10] = { "", "сто", "двести", "триста", "четыреста", "пятьсот", "шестьсот", "семьсот", "восемьсот", "девятьсот" };

std::string convert_triplet(int n, bool female) {
    if (n == 0) return "";

    std::string result;
    int s = n / 100;
    int d = (n / 10) % 10;
    int e = n % 10;

    if (s > 0) {
        result = sotni[s];
    }

    if (d == 1) {
        if (!result.empty()) result += " ";
        result += desyatki1[e];
    }
    else {
        if (d > 1) {
            if (!result.empty()) result += " ";
            result += desyatki2[d];
        }
        if (e > 0) {
            if (!result.empty()) result += " ";
            result += female ? edinici_zh[e] : edinici[e];
        }
    }

    return result;
}

std::string rubley_form(int n) {
    if (n % 100 >= 11 && n % 100 <= 19) return "рублей";

    int last = n % 10;
    if (last == 1) return "рубль";
    if (last >= 2 && last <= 4) return "рубля";
    return "рублей";
}

int main() {
    system("chcp 1251 > nul");

    int summa;
    std::cout << "Введите сумму в рублях: ";
    std::cin >> summa;

    if (summa < 0 || summa > 1000000000) {
        std::cout << "Сумма должна быть от 0 до 1 000 000 000" << std::endl;
        return 1;
    }

    if (summa == 0) {
        std::cout << "ноль рублей" << std::endl;
        return 0;
    }

    std::string result;
    int ml = summa / 1000000;
    int th = (summa / 1000) % 1000;
    int ost = summa % 1000;

    if (ml > 0) {
        result = convert_triplet(ml, false);
        if (ml % 100 >= 11 && ml % 100 <= 19) result += " миллионов";
        else if (ml % 10 == 1) result += " миллион";
        else if (ml % 10 >= 2 && ml % 10 <= 4) result += " миллиона";
        else result += " миллионов";
    }

    if (th > 0) {
        if (!result.empty()) result += " ";
        result += convert_triplet(th, true);
        if (th % 100 >= 11 && th % 100 <= 19) result += " тысяч";
        else if (th % 10 == 1) result += " тысяча";
        else if (th % 10 == 2) result += " тысячи";
        else if (th % 10 == 3 || th % 10 == 4) result += " тысячи";
        else result += " тысяч";
    }

    if (ost > 0) {
        if (!result.empty()) result += " ";
        result += convert_triplet(ost, false);
    }

    result += " " + rubley_form(summa);

    std::cout << result << std::endl;

    return 0;
}
