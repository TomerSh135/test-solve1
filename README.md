using static TestSolve.Class1;
using static TestSolve.Q1;

namespace TestSolve
{
    class Program
    {
        static void Main(string[] args)
        {
            Customer c1 = new Customer("Alice", 6);
            Customer c2 = new Customer("Bob", 3);
            Customer c3 = new Customer("Charlie", 10);

            Node<Call> head = new Node<Call>(new Call(c1, 120));
            head.SetNext(new Node<Call>(new Call(c2, 90)));
            head.GetNext().SetNext(new Node<Call>(new Call(c3, 150)));

            CallCenter callCenter = new CallCenter(head);

            Customer maxVipCustomer = callCenter.MaxVIPWait();

            if (maxVipCustomer != null)
            {
                Console.WriteLine($"The VIP customer with the longest wait time is: {maxVipCustomer.Name}");
            }
            else
            {
                Console.WriteLine("No VIP customer is waiting.");
            }
        }

        public static Node<Call> GenerateUrgent(Node<Call> head, int threshold)
        {
            Node<Call> dummy = new Node<Call>(new Call(new Customer("alon", 4), 200));
            Node<Call> current = head;


            if (head == null)
                return null;

            while (current != null)
            {
                Call currentCall = current.GetValue();
                if (currentCall.Seconds > threshold)
                {
                    dummy.SetNext(new Node<Call>(current.GetValue()));

                }
                else if (currentCall.Customer.IsVIP())
                {
                    dummy.SetNext(new Node<Call>(current.GetValue()));
                }

                current = current.GetNext();
            }


            return dummy.GetNext();
        }

        public static void PrintCalls(Node<Call> head)
        {
            Node<Call> current = head;
            while (current != null)
            {
                Call call = current.GetValue();
                Console.WriteLine($"Customer: {call.Customer.Name}, VIP: {call.Customer.IsVIP()}, Wait Time: {call.Seconds} seconds");
                current = current.GetNext();
            }
        }
    }
}


using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using static TestSolve.Q1;

namespace TestSolve
{
    internal class Q1
    {
        public class Node<T>
        {
            private T value;
            private Node<T> next;

            public Node(T value)
            {
                this.value = value;
                this.next = null;
            }
            public Node(T value, Node<T> next)
            {
                this.value = value;
                this.next = next;
            }
            public T GetValue()
            {
                return this.value;
            }
            public Node<T> GetNext()
            {
                return this.next;
            }
            public void SetValue(T value)
            {
                this.value = value;
            }
            public void SetNext(Node<T> next)
            {
                this.next = next;
            }
            
            public bool HasNext()
            {
                return (this.next != null);
            }
           
            public override string ToString()
            {
                return value.ToString();
            }
        }

        public class Customer
        {
            public string Name { get; set; }
            public int YearsAsMember { get; set; }

            public Customer(string name, int yearsAsMember)
            {
                Name = name;
                YearsAsMember = yearsAsMember;
            }

            public Customer(Customer other)
            {
                Name = other.Name;
                YearsAsMember = other.YearsAsMember;
            }

            public bool IsVIP()
            {
                return YearsAsMember > 5;
            }
        }

        public class Call
        {
            public Customer Customer { get; set; }
            public int Seconds { get; set; }

            public Call(Customer customer, int seconds)
            {
                Customer = customer;
                Seconds = seconds;
            }

            public Call(Call other)
            {
                Customer = new Customer(other.Customer);
                Seconds = other.Seconds;
            }
        }
        
       

        

    

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using static TestSolve.Q1;

namespace TestSolve
{
    internal class Class1
    {
       
    
        public class CallCenter
        {
            private Node<Call> callList;
            public CallCenter(Node<Call> callList)
            {
                this.callList = callList;
            }

            public Node<Call> GetCallList()
            {
                return callList;
            }

            public Customer MaxVIPWait()
            {
                int maxVip = -1;
                Node<Call> c = this.callList;
                Customer maxVipCustomer = null;

                while (c != null)
                {
                    if (c.GetValue().Customer.YearsAsMember > 5)
                    {
                        if (c.GetValue().Seconds > maxVip)
                        {
                            maxVip = c.GetValue().Seconds;
                            maxVipCustomer = c.GetValue().Customer;
                        }
                    }
                    c = c.GetNext();
                }

                return maxVipCustomer;
            }
        }
    }
}
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace TestSolve
{
    internal class Q2
    {
        public class Event
        {
            private Date date;
            private int hour;

            public Date getDate()
            {
                return this.date;
            }
            public virtual bool Match(string name)
            {
                return false;
            }
        }
        public class Meeting : Event
        {
            private string[] arrNames;
            private string location;
            private int duration;

            public override bool Match(string name)
            {
                for (int i = 0; i < arrNames.Length; i++)
                {
                    if (arrNames[i] == name)
                    {
                        return true;
                    }
                }
                return false;
            }
        }
        public class PhoneCall : Event
        {
            private string phoneNumber;
            private string name;

            public override bool Match(string name)
            {
                return name == this.name;
            }
        }
        public class Task : Event
        {
            private int hour;
            private string title;
        }
        public class Date
        {
            private int day;
            private int month;
            private int year;

            public bool Same(Date other)
            {
                return (this.day == other.day && this.month == other.month && this.year == other.year);
            }
        }
        public class Diary
        {
            private Event[] arr;

            public PhoneCall[] AllCalls(Date newDate)
            {
                PhoneCall[] Calls = new PhoneCall[100];
                int index = 0;

                for (int i = 0; i < arr.Length; i++)
                {
                    if (arr[i].getDate().Same(newDate))
                    {
                        Calls[index] = (PhoneCall)arr[i];
                        index++;
                    }
                }
                return Calls;
            }
        }

    }
}
