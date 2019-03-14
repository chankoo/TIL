#### 19.03.14

## 예제 코드
    #include <iostream>
    #include <string>

    using namespace std;

    Class Student{
        private:
            string name;
            int score;

        public:
            Student(string name,int score){
                this->name = name;
                this->score = score;
            }
            Student(const Student& other){
                name = other.name;
                score = other.score;
            }
            ~Student(){
                cout << "객체가 소멸됩니다\n";
            }
            void scoreUp(){
                score++
            }
            void show(){
                cout << name << score << endl;
            }
    }

## this 포인터


## 복사생성자

해당 클래스의 다른 인스턴스를 생성자의 매개변수로 받아서 인스턴스를 생성하는 생성자

    Student s1 = Student("Alice",90);
    s1.scoreUp()
    Student s2 = Student(s1) // 복사생성자 이용
    s1.show()
    s2.show() // s2의 score가 s1 보다 1 높음

## 객체 생성과 소멸에 동적할당 이용과 그렇지 않을 경우 비교

- 동적할당의 경우
  - 생성: `Student* s1 = new Student("Alice",90);`
  - 메소드 호출: `s1->scoreUp();`
  - 소멸: `delete s1;`

- 일반적인 생성의 경우
  - 생성: `Student s2 = Student("Bob",90);`
  - 메소드 호출: `s2.scoreUp()`
  - 소멸: `delete s2;` 시 오류

