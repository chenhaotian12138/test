# ����ѧ����Ϣ����ϵͳ
## 1��ϵͳ���
- #### ͨ��C#����SQL Server�е����ݿ⣬ʵ�ֶ�ѧ����Ϣ������ɾ���ģ���ѯ�ȹ��ܡ�
## 2��׼������
- #### windows 10ϵͳ
- #### ��װ [Visual Studio](https://developer.microsoft.com/en-us/windows/downloads) (community �汾).
- #### ��װ[SQL Server](https://docs.microsoft.com/zh-cn/sql/sql-server/sql-server-technical-documentation?toc=toc.json&view=sql-server-2017)
- #### Visual Studio[��װ����](tps://www.cnblogs.com/whuanle/p/8507478.html)
- #### SQL Server [��װ����](https://www.cnblogs.com/ios9/p/9527939.html)
## 3���������
### 3.1 ����˼·
- #### 1����SQL Server�д���һ�����ݿ⣬�����ݿ��д����������ѧ����Ϣ��ѧ�ţ��������Ա����䣬�༶��
- #### 2����C#�д�������������ʵ�������ݿ�ĶԽ�
- #### 3����C#������if���ֱ�д������ɾ���ģ���ѯģ�����
- #### 4����C#�д���һ���ж��Ƿ��Ѵ��ڴ�ѧ����Ϣ�ĺ���
### 3.2 �����������

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data;
using System.Data.SqlClient;
namespace ConsoleApp3
{
    class Program
    {
      //�ж��Ƿ��Ѿ����ڴ�ѧ����Ϣ�ĺ���
        public static bool Exists(SqlConnection conn, string stu_no, string class_name)
          {
            string strSQL = "SELECT * FROM students WHERE ѧ��='" + stu_no + " 'AND �༶= '" + class_name + "'";
            SqlCommand comm = new SqlCommand(strSQL, conn);
            SqlDataReader dr = comm.ExecuteReader();
            
            if (dr.Read())
            {
                dr.Close();
                return true;
            }
            else
            {
                dr.Close();
                return false;
            }
          }
        static void Main(string[] args)
        {
           //��������
            SqlConnectionStringBuilder scsb = new SqlConnectionStringBuilder();
            scsb.DataSource = "127.0.0.1";
            scsb.IntegratedSecurity =true;
            scsb.InitialCatalog = "stu_db";
             //���������ж��Ƿ��Ѿ�������
            SqlConnection conn = new SqlConnection(scsb.ToString());
            if (conn.State == ConnectionState.Closed)
            { conn.Open(); }
            while (true)
            {
                Console.WriteLine("��������еĲ��������ѧԱ����1���޸�ѧԱ����2��ɾ��ѧԱ����3����ѯѧԱ����4");
                string type = Console.ReadLine();
                string stu_no = "null";
                string class_name = "null";
                string stu_name = "null";
                string stu_sex = "null";
                string strSQL= "null";
                int stu_age = 0;
                bool flag;
                //1����ģ��
                if (type == "1")
                {
                    Console.WriteLine("������ѧ��");
                    stu_no = Console.ReadLine();
                    Console.WriteLine("������༶");
                    class_name = Console.ReadLine();
                    //�ж�
                    flag = Exists(conn, stu_no, class_name);
                    if (flag)
                    {
                        Console.WriteLine("����ѧԱ����Ϣ�Ѿ����ڣ�");
                        continue;
                    }
                    else
                    {

                        Console.WriteLine("����������");
                        stu_name = Console.ReadLine();
                        Console.WriteLine("�������Ա�");
                        stu_sex = Console.ReadLine();
                        Console.WriteLine("����������");
                        stu_age = int.Parse(Console.ReadLine());

                        //�������
                        string strInsert = "INSERT INTO [dbo].[students]([ѧ��],[�༶] ,[����] ,[�Ա�] ,[����]) VALUES('" + stu_no + " ', '" + class_name + "', '" + stu_name + "', '" + stu_sex + "'," + stu_age + ")";

                        //Console.WriteLine(strInsert);

                        //�������ݱ�
                        SqlCommand comm = new SqlCommand(strInsert, conn);
                        //ִ��

                        try
                        {
                            int rc = comm.ExecuteNonQuery();
                            if (rc == 1)
                            {
                                Console.WriteLine("ѧԱ��ӳɹ�");
                            }
                            else
                            {
                                Console.WriteLine("ѧԱ���ʧ��");
                            }
                        }
                        catch (Exception ex)
                        {
                            Console.WriteLine(ex.Message);
                        }
                    }
                }
                //2����ģ��
                else if (type == "2")
                {
                    Console.WriteLine("������ѧ��");
                    stu_no = Console.ReadLine();
                    Console.WriteLine("������༶");
                    class_name = Console.ReadLine();
                    //�ж�
                    flag = Exists(conn, stu_no, class_name);
                    if (!flag)
                    {
                        Console.WriteLine("����ѧԱ����Ϣ�����ڣ�");
                        continue;
                    }
                    else
                    {
                        Console.WriteLine("����������");
                        stu_name = Console.ReadLine();
                        Console.WriteLine("�������Ա�");
                        stu_sex = Console.ReadLine();
                        Console.WriteLine("����������");
                        stu_age = int.Parse(Console.ReadLine());
                        //�������
                        string strupdate = " UPDATE [dbo].[students] SET [����] = '" + stu_name + " ',[�Ա�] = '" + stu_sex + " ' ,[����] =" + stu_age + " WHERE ѧ��='" + stu_no + " 'AND �༶='" + class_name + "'";
                        // Console.WriteLine(strupdate);

                        //�������ݱ�
                        SqlCommand comm = new SqlCommand(strupdate, conn);
                        //ִ��

                        try
                        {
                            int rc = comm.ExecuteNonQuery();
                            if (rc == 1)
                            {
                                Console.WriteLine("ѧԱ�޸ĳɹ�");
                            }
                            else
                            {
                                Console.WriteLine("ѧԱ�޸�ʧ��");
                            }
                        }
                        catch (Exception ex)
                        {
                            Console.WriteLine(ex.Message);
                        }
                    }
                }
                 //3��ɾģ��
                else if (type == "3")
                {
                    Console.WriteLine("������ѧ��");
                    stu_no = Console.ReadLine();
                    Console.WriteLine("������༶");
                    class_name = Console.ReadLine();
                    //�ж�
                    flag = Exists(conn, stu_no, class_name);
                    if (!flag)
                    {
                        Console.WriteLine("����ѧԱ����Ϣ�����ڣ�");
                        continue;
                    }
                    else
                    {

                        //�������
                        string strdelete = "DELETE FROM [dbo].[students] WHERE ѧ��='" + stu_no + "'AND �༶='" + class_name + "'";
                        // Console.WriteLine(strupdate);

                        //�������ݱ�
                        SqlCommand comm = new SqlCommand(strdelete, conn);
                        //ִ��

                        try
                        {
                            int rc = comm.ExecuteNonQuery();
                            if (rc == 1)
                            {
                                Console.WriteLine("ѧԱɾ���ɹ�");
                            }
                            else
                            {
                                Console.WriteLine("ѧԱɾ��ʧ��");
                            }
                        }
                        catch (Exception ex)
                        {
                            Console.WriteLine(ex.Message);
                        }
                    }
                }
                //4����ѯģ��
                else if (type == "4")
                {
                    Console.WriteLine("������ѧ��");
                    stu_no = Console.ReadLine();
                    Console.WriteLine("������༶");
                    class_name = Console.ReadLine();
                    //�ж�
                    flag = Exists(conn, stu_no, class_name);
                    if (!flag)
                    {
                        Console.WriteLine("����ѧԱ����Ϣ�����ڣ�");
                        continue;
                    }
                    else
                    {


                        //�������
                        string str = "SELECT * FROM students WHERE ѧ��='" + stu_no + " '";
                        // Console.WriteLine(strupdate);

                        //�������ݱ�
                        SqlCommand comm = new SqlCommand(str, conn);
                        //ִ��
                        SqlDataReader sdr = null;
                        try
                        {
                            sdr = comm.ExecuteReader();
                            //��ȡ����
                            while (sdr.Read())
                            {
                                Console.WriteLine("����" + sdr["����"]);
                                Console.WriteLine("�༶" + sdr["�༶"]);
                                Console.WriteLine("�Ա�" + sdr["�Ա�"]);
                                Console.WriteLine("����" + sdr["����"]);
                                Console.WriteLine("-------------------");
                            }
                        }
                        catch (Exception ex)
                        {
                            Console.WriteLine(ex.Message);
                        }
                    }
                }
                
                    Console.ReadLine();
            }

        }
        
    }
 }
```
## 4��������ʾ 
#### ����Ϊ����ѧ����Ϣ������ʾ��
<img src="1.png" alt="���ѧ����Ϣ" height="300" width="1200" >
<img src="2.jpg" alt="��ӽ��" height="300" width="1000" >
<img src="3.png" alt="�ظ����ʱ" height="300" width="1000" >

#### ����ɾ�����޸ģ���ѯ�������
## 5����Ŀ����
#### �º���