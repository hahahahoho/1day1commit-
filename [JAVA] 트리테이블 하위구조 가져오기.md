# 트리구조의 데이터를 처리하기 위한 하위구조가져오기

```java
public static String[] tbl_name = { "tbl_dept_tree_t1", "tbl_dept_tree_t2", "tbl_dept_tree_t3", "tbl_dept_tree_t4", "tbl_dept_tree_t5",
			"tbl_dept_tree_t6", "tbl_dept_tree_t7", "tbl_dept_tree_t8", "tbl_dept_tree_t9", "tbl_dept_tree_t10", "tbl_dept_tree_t11",
			"tbl_dept_tree_t12", "tbl_dept_tree_t13", "tbl_dept_tree_t14", "tbl_dept_tree_t15", "tbl_dept_tree_t16", "tbl_dept_tree_t17",
			"tbl_dept_tree_t18", "tbl_dept_tree_t19", "tbl_dept_tree_t20" };
	public static String[] col_name = { "t1", "t2", "t3", "t4", "t5", "t6", "t7", "t8", "t9", "t10", "t11", "t12", "t13", "t14", "t15", "t16", "t17",
			"t18", "t19", "t20" };

	public ArrayList<String> getdept(String dept_cd) {
		Connection connect = connector.connect();
		String target = dept_cd;
		ArrayList<String> target_arr = new ArrayList<>();
		int index = 0;
		target_arr.add(target);
		boolean flag = true;
		try {
			for (int i = 0; i < tbl_name.length; i++) {
				int count = find_tbl(tbl_name[i], col_name[i], target, connect);
				if (count != 0) {
					index = i;
					break;
				}
				// loop돌렸을 때 해당 dept_cd없으면 빈배열 return
				if (i == tbl_name.length - 1 && count == 0) {
					flag = false;
					target_arr.clear();
					// return target_arr;
				}
			}
			if (flag) {
				for (int j = index + 1; j < col_name.length; j++) {
					String my_tbl_name = tbl_name[j];
					String target_col = col_name[index];
					int count = find_child_cd(my_tbl_name, col_name[j], target, target_col, connect, target_arr);
					if (count == 0) {
						break;
					}
				}
			}
		} catch (Exception e) {
			// TODO: handle exception
		}
		return target_arr;
	}

	public Integer find_tbl(String tbl_name, String col_name, String target, Connection connect) throws Exception {
		String sql = "SELECT COUNT(" + col_name + ") FROM " + tbl_name + " WHERE " + col_name + "='" + target + "'";
		Statement stmt = connect.createStatement();
		ResultSet rs = stmt.executeQuery(sql);
		int count = 0;
		if (rs.next()) {
			count = rs.getInt(1);
		}
		return count;
	}

	public Integer find_child_cd(String my_tbl_name, String col_name, String target, String target_col, Connection connect,
			ArrayList<String> target_arr) throws Exception {
		String sql_c = "SELECT COUNT(" + col_name + ") FROM " + my_tbl_name + " WHERE " + target_col + "='" + target + "'";
		Statement stmt_c = connect.createStatement();
		ResultSet rs_c = stmt_c.executeQuery(sql_c);
		int result = 0;
		if (rs_c.next()) {
			int count = rs_c.getInt(1);
			if (count != 0) {
				String sql = "SELECT DISTINCT " + col_name + " FROM " + my_tbl_name + " WHERE " + target_col + "='" + target + "'";
				Statement stmt = connect.createStatement();
				ResultSet rs = stmt.executeQuery(sql);
				while (rs.next()) {
					target_arr.add(rs.getString(1));
				}
				result = 1;
			}
		}
		return result;
	}
```

