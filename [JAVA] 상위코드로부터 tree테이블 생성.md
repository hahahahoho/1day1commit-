# 상위코드를 통하여 tree구조 테이블 생성

```java
package whitedefense.deptmigration;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.DriverManager;
import java.util.*;
import java.lang.*;

public class main {

	public static Connection src_conn = null;
	public static Connection dest_conn = null;

	public static String cmd = null;
	public static String db_ip 	= null;
	public static String db_port 	= null;
	public static String db_name	= null;
	public static String db_tbl	= null;
	public static String db_id	= null;
	public static String db_pwd	= null;
	
	public static String target_ip 	= null;
	public static String target_port 	= null;
	public static String target_name	= null;	
	public static String target_tbl	= null;
	public static String target_id	= null;
	public static String target_pwd	= null;
	
	public static String val = null;
	
	public static void BaseQuery(String sql) throws SQLException
	{	
		Statement stmt = src_conn.createStatement();		
		try {
			stmt.executeUpdate(sql);
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	
	public static ArrayList<String> ReturnQuery(String dept_val) throws SQLException
	{	
		ArrayList<String> dept_list = new ArrayList<String>();
				
		//tbl_dept_info_new
		String sql = "SELECT dept_cd FROM " + db_tbl + " WHERE uppr_dept_cd = '"+ dept_val + "';";
		
		Statement stmt = src_conn.createStatement();		
		ResultSet rs = null;
		try {
			rs = stmt.executeQuery(sql);
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		int no = 0;		
		while(rs.next()) {
			//System.out.println(no+1 + " : " +  rs.getString(1));			
			dept_list.add(rs.getString(1).toString());			
			no++;
		}		
		return dept_list;		
	}

	public static int tree_num = 0 ;
	public static String t_val="";
	public static String tbl_name="";
	public static String fld_name="";
	public static String t="";			
	public static String sql = "";

	
	public static int RecursiveQuery(int t_num, String dept_val, ArrayList<String> SubDept) throws SQLException
	{	
		int ReturnVal=0;

		ArrayList<String> dept_list = new ArrayList<String>();		

		dept_list = ReturnQuery(dept_val);
		
		//System.out.println(t_num + " Arr Size " + dept_list.size());
		
		t_num++;
		fld_name = "";
		t_val = "";
		
		tree_num++;

		for(String obj:dept_list)
		{
			//System.out.println("t" + String.valueOf(tree_num) + " : " + obj);
			
			for(int i = 0 ; i < t_num ; i++)
			{
				fld_name += "t" + String.valueOf((i+1)) + "," ;
				
			}
			
			SubDept.add(obj);

			//System.out.println("dbg => t_num : " + t_num + " , size : " + SubDept.size());
			
			if(SubDept.size() > t_num)
			{
				SubDept.set(t_num-1,  obj);

				for(int d = t_num ; d < SubDept.size()  ; d++)
				{
					//System.out.println("remove : " + SubDept.get(d-1));					
					SubDept.remove(d);
				}
			}

			tbl_name = target_tbl + String.valueOf(t_num);
			
			//System.out.println("dbg => t_num : " + t_num + " , size : " + SubDept.size());
				
			//for(int j = 0 ; j < SubDept.size()  ; j++) //<==이렇게 하면 가비지를 제거하기 어려움
			for(int j = 0 ; j < t_num  ; j++)
			{	
				t_val += "'" + SubDept.get(j) + "',";				
			}

			t_val = t_val.substring(0, t_val.length()-1);
			fld_name = fld_name.substring(0, fld_name.length()-1);
			
			System.out.println("---------------------------------------");
			System.out.println(t_val);
			
			System.out.println(tbl_name + " : " + t_num + " : " + fld_name + " : " + obj);
			
			sql = "INSERT INTO " + tbl_name + "(" + fld_name + ") VALUES (" + t_val + ")";
			System.out.println(sql);
			InsertQuery(sql);
			
			ReturnVal = RecursiveQuery(t_num,  obj, SubDept);
			
			System.out.println("---------------------------------------");
			//SubDept.clear();
		}

		//fld_name =  "";
		//tree_num++;

		return dept_list.size();
	}
	
	//public static void InsertQuery(String t_num, String data) throws SQLException
	public static void InsertQuery(String sql) throws SQLException
	{	
		//String sql = "INSERT INTO " + target_tbl + t_num + "(val) VALUES ('" + data + "')";
		//System.out.println(sql);		

		Statement stmt = dest_conn.createStatement();		
		try {
			stmt.executeUpdate(sql);
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	
	
	public static void main(String[] args) throws ClassNotFoundException, SQLException
	{
		Class.forName("cubrid.jdbc.driver.CUBRIDDriver");
				
		if(args!=null)
        {
			for(int i=0; i<args.length; i++)
			{
				System.out.println("args["+i+"]="+args[i]);
			}

			if(args.length > 8)
			{
				cmd 	= args[0];
				db_ip 	= args[1];
				db_port = args[2];
				db_name	= args[3];
				db_id	= args[4];
				db_pwd	= args[5];
				db_tbl	= args[6];
	
				target_ip = args[7];
				target_port	= args[8];
				target_name	= args[9];
				target_id	= args[10];
				target_pwd	= args[11];
				target_tbl	= args[12];
				
				val	= args[13];
			}
		}

		if(
				db_ip !=null &&
				db_port !=null &&
				db_name !=null &&
				db_id !=null &&
				db_pwd !=null &&
				db_tbl !=null &&
				target_ip !=null &&
				target_port !=null &&
				target_name !=null &&
				target_id !=null &&
				target_pwd !=null &&
				target_tbl !=null &&
				val !=null
				)
		{			
			
			System.out.println("src : " + db_ip + "," + db_port + "," + db_name+ "," + db_tbl+"," + db_id + "," + db_pwd);
			System.out.println("dest : " + target_ip + "," + target_port + "," + target_name+ "," + target_tbl+ "," + target_id+ "," + target_pwd);
			System.out.println("top value : " + val);
			
			//conn = DriverManager.getConnection("jdbc:CUBRID:175.207.13.165:30000:whitedefense:::?charSet=utf8", "dba", "");
			src_conn = DriverManager.getConnection(
					"jdbc:CUBRID:"+db_ip+":" + db_port+":"+db_name +":::?charSet=utf8", db_id, db_pwd);
			
			dest_conn = DriverManager.getConnection(
					"jdbc:CUBRID:"+target_ip+":" + target_port+":"+target_name +":::?charSet=utf8", target_id, target_pwd);
			
			/*
			dept_list = Query("23RHKJFEW9Y8E9FWY8NLK3R243R2U9");
			Iterator itr=dept_list.iterator();
			while(itr.hasNext()){  		
				System.out.println(itr.next());
			}  
			*/
			/*
			int alter_num=1;		
			while(alter_num < 20)
			{
				String s = "ALTER TABLE tbl_dept_tree_info ADD ( t"+ String.valueOf(alter_num) +" VARCHAR(160));";
				BaseQuery(s);
				alter_num++;
			}
			*/
			if(cmd.compareTo("create") == 0)
			{
				String tbl_name="";
				String t="";
				for(int i=1 ; i < 20 ; i++)
				{
					tbl_name = target_tbl + String.valueOf(i);
					t += "t"+ String.valueOf(i) + " VARCHAR(1024)";				
					String sql = "CREATE TABLE " + tbl_name + "(" + t + ")";
					//System.out.println(tbl_name + " : " + t);
					System.out.println(sql);
					BaseQuery(sql);	
					t += ",";
				} //end if
			}
			if(cmd.compareTo("non") == 0)
			{
				ArrayList<String> dept_list = new ArrayList<String>();	
				RecursiveQuery(0, val, dept_list);//"23RHKJFEW9Y8E9FWY8NLK3R243R2U9");		

				/*
				int i=1;
				for(String s : dept_list) 
				{
					System.out.println(String.valueOf(i) + " : " + s); 
					i++;
				}
				*/
			}
		}
		else
		{
			System.out.println("다음과 같이 12개의 args 를 넣어줘야함!");
			System.out.println("{create or non}  {소스 : ip port dbname id pwd tbl_dept_info } {타겟 : ip port dbname id pwd tbl_dept_tree_t } uppr_dept_cd");
		}
		
	}

}

```

