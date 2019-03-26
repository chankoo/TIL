#### 19.03.14
#### 19.03.26

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

## iostream 헤더파일
c++에서 표준 입출력에 필요한 것들(cout, endl 등)을 포함
- cout
    - `ostream` 클래스의 객체로 표준출력을 담당
    - `operator<<` 가 `ostream` 클래스에 정의된 연산자이기에 
    - `cout << 'hi';` 와 같은 작업이 가능함
    - `operator<<` 가 인자로 들어오는 각각의 자료형에 대해 오버로딩돼있기에 어떤 자료형에 대해서도 `cout` 사용 가능
- endl
    - '엔터 하나'를 출력하는 '함수' `cout << endl;` 으로 출력가능

## namespace
말그대로 특정한 공간에 이름을 붙인 것
- cout, endl 등은 iostream 헤더파일의 `std`라는 namespace에 정의되었기에 `using namespace std`로 간편하게 사용
- namespace에 대해 좀더 알아보자

    `header1.h`과 `header2.h`에 `function`이라는 함수가 동시에 정의되어있는 상황을 가정해보자
    C언어에서는 이러한 문제를 해결하는 방법이 없었다. function의 이름을 바꾸는 방법 뿐임

    C++에서는 namespace를 도입하여 이 문제에 대한 유연한 해결책을 제공한다
    namespace를 이용해보자
> // header1.h 의 내용
>
> namespace header1 {
>     
> int function(); }
> 

> // header2.h 의 내용
>
> namespace header2 {
> 
> int function(); }

    헤더 각각의 function은 각기 다른 namespace에 공간에 속한다
    따라서 `header1::function();`
    `header2::function();`
    와 같이 이용 가능하다
    이때 `using namespace header1;`이라고 선언한다면
    그냥 `function()`만으로도 header1의 function을 갖다 쓸 수 있게 된다

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

