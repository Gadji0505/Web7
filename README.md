import java.util.Scanner;

class MyException extends Exception
{
String name;
int d,m,y;

     MyException(String name,int d,int m,int y) {
         super(name);
         this.d=d;
         this.m=m;
         this.y=y;
    }
String getDate(){
return d+"."+m+"."+y;
}
}

 class Data {
int d,m,y;
Data (int d,int m,int y) throws MyException
{
    if(d>31||m>12||y<0||d<0||m<0) throw new MyException("Некорректная дата",d,m,y);
 this.d=d;
 this.m=m;
 this.y=y;
}
// добавить метод вывода данных
// метод - прибавить к дате заданное кол-во дней с получением новой даты
// метод-вычислить номер дня от начала года
// вычислит кол дней между двумя датами 
// вычит из даты n кол дней и получение новой даты
}
public class ExceptionsMy {

    
    public static void main(String[] args) {
       Scanner sc=new Scanner (System.in);
       System.out.println("Введите день");
        int d=sc.nextInt(); 
        System.out.println("Введите месяц");
        int m=sc.nextInt();  
        System.out.println("Введите год");
        int y=sc.nextInt();  
        try(
                Data date=new Data(d,m,y);
                //вызов методов
            }
        catch ( MyException e)
        {System.out.println(e.getMessage()+" "+e.getDate());}
    
}
}