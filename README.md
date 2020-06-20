C# 实现顺序链表
共有三个类，分别是：
1.接口类
2.实现类
3.测试类
具体代码如下：

1.接口类

/// <summary>
    /// 该类为链表接口定义类，顺序链表和单链表都使用该接口
    /// </summary>
    /// <typeparam name="T"></typeparam>
    interface IListDS<T>
    {
        /// <summary>
        /// 获取链表长度
        /// </summary>
        /// <returns></returns>
        int GetLength();

        /// <summary>
        /// 清空链表
        /// </summary>
        void Clear();

        /// <summary>
        /// 判断链表是否为空
        /// </summary>
        /// <returns></returns>
        bool IsEmpty(); 

        /// <summary>
        /// 向链表中添加元素
        /// </summary>
        /// <param name="item"></param>
        void Add(T item);

        /// <summary>
        /// 根据索引向链表中插入元素
        /// </summary>
        /// <param name="item"></param>
        /// <param name="index"></param>
        void Insert(T item, int index);

        /// <summary>
        /// 根据索引删除链表中的元素
        /// </summary>
        /// <param name="index"></param>
        /// <returns></returns>
        T Delete(int index);

        /// <summary>
        ///根据索引获取元素值
        /// </summary>
        /// <param name="index"></param>
        /// <returns></returns>
        T GetEle(int index);

        /// <summary>
        /// 更具元素值，获取元素在链表中的位置
        /// </summary>
        /// <param name="value"></param>
        /// <returns></returns>
        int Locate(T value);

        /// <summary>
        /// 格局索引获取元素值
        /// </summary>
        /// <param name="index"></param>
        /// <returns></returns>
        T this[int index] { get; }
    }
    
    
    
    2.实现类
    
     /// <summary>
    /// 顺序表实现方式，该顺序链表长度不会自增链表
    /// </summary>
    /// <typeparam name="T"></typeparam>
    /// 
    class SeqList<T> : IListDS<T>                   //数组类型位置，所以SeqList使用T来传入参数类型，该类实现接口类IListDS类
    {

        private T[] data;                           //定义一个数组data，用来存放数据
        private int count = 0;                      //定义一个变量，用来表示顺序链表元素个数

        public SeqList(int size)                    //构造函数初始化，size指定数组data长度,数组初始，元素个数为零
        {                                           //这里定义的数组长度不会变化，即顺序链表长度不会变化
            data = new T[size];
            count = 0;
        }
        public SeqList() : this(10) { }             //这里指定数组长度为10，即顺序链表长度为10

        #region 根据索引获取值
        public T this[int index]                    //根据索引index获取元素值
        {
            get { return GetEle(index); }           //调用当前类的GetEle方法获取元素值
        }
        #endregion

        #region 向顺序链表中添加元素
        public void Add(T item)                     //向顺序链表中添加一个元素
        {
            if (count == data.Length)               //判断顺序链表中元素个数是否与数组大小相等，如果相等，说明数组（链表）存满了
            {
                Console.WriteLine("链表已存满，不允许在存入"); 
            }
            else
            {
                data[count] = item;
                count++;
            }
        }
        #endregion

        #region 清空顺序链表
        public void Clear()                          //清空顺序链表，将元素个数设置为0即可
        {
            count = 0;
        }
        #endregion

        #region 根据索引删除元素
        public T Delete(int index)                   //根据索引删除元素,并将删除的元素返回，需要将索引之后的元素向前移动一个位置
        {
            T temp = data[index];                    //创建一个临时变量，用来存放要删除的元素
            for(int i = index; i < count; i++)
            {
                data[i] = data[i + 1];               //循环遍历，将要删除的元素后面的数据向前移动一个位置

            }
            count--;                                 //将元素个数减一
            return temp;
        }
        #endregion


        #region 根据索引获取值
        public T GetEle(int index)                   //根据索引获取元素
        {
            if (index >= 0 && index <= count - 1)    //判断元素索引index是否小于元素个数count，小于则存在该元素，大于则索引越界
            {
                return data[index];
            }
            else
            {
                Console.WriteLine("索引越界");
                return default;
            }
        }
        #endregion

        #region 获取顺序链表中元素个数
        public int GetLength()                       //获取顺序链表长度，即元素个数
        {
            return count;
        }
        #endregion

        #region 向顺序链表中插入元素
        public void Insert(T item, int index)        //向顺序链表中插入一个元素
        {
            if (count + 1 < data.Length)             //判断如果插入后，元素个数是否大于数组长度，如果大于，则链表存满，不能插入
            {
                for (int i = count; index <= i; i--) //循环遍历，将插入位置及其之后的元素向后移动一个位置
                {
                    data[i] = data[i - 1];
                }
                data[index] = item;                  //根据索引将元素插入到数组中
                count++;
            }
            else
            {
                Console.WriteLine("链表已满，不能再插入数据");
            }
            
        }
        #endregion

        #region 判断顺序链表是否为空
        public bool IsEmpty()                         //判断顺序链表是否为空，即元素个数是否为零
        {
            return count == 0;
        }
        #endregion

        #region 根据值获取元素在链表中的位置
        public int Locate(T value)                    //根据元素值，获取元素在顺序链表中的位置
        {
            for(int i = 0; i < count; i++)
            {
                if (data[i].Equals(value))            //循环遍历数组，根据元素值，获取该元素在链表（数组）中的位置
                {
                    return i;
                }
            }
            return -1;                                 //如果该元素不在链表（数组）中，返回-1
        }
        #endregion
    }
    
    
    3.测试类
    
    static void Main(string[] args)
    {
      IListDS<string> list = new SeqList<string>();
            list.Add("1");
            list.Add("2");
            list.Add("3");
            list.Add("4");
            list.Add("5");
            list.Add("6");
            list.Add("7");
            list.Add("8");
            list.Add("9");
            list.Add("10");
            list.Insert("&", 3);
            Console.WriteLine(list.GetLength());
            Console.WriteLine("--------------");
            for (int i = 0; i < list.GetLength() - 1; i++)
            {
                Console.Write(list.GetEle(i) + " ");
            }
    }
