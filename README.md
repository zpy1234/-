# -*
 2  * To change this license header, choose License Headers in Project Properties.
 3  * To change this template file, choose Tools | Templates
 4  * and open the template in the editor.
 5  */
 6 package cn.itcast.dao.impl;
 7
 8 import cn.itcast.dao.UserDao;
 9 import cn.itcast.pojo.User;
10 import java.io.BufferedReader;
11 import java.io.BufferedWriter;
12 import java.io.File;
13 import java.io.FileReader;
14 import java.io.FileWriter;
15 import java.io.IOException;
16 /**
17  *
18  * @author Administrator
19  */
20 public class UserDaoImpl implements UserDao {
21
22     //定义文件
23     private static File file = new File("user.txt");
24
25     //类加载的时候就把文件创建
26     static {
27         try {
28             file.createNewFile();
29         } catch (IOException ex) {
30             ex.printStackTrace();
31         }
32     }
33
34     @Override
35     public boolean login(String username, String password) {
36         boolean flag = false;
37
38         BufferedReader br = null;
39         try {
40             br = new BufferedReader(new FileReader(file));
41             String line = null;
42             while ((line = br.readLine()) != null) {
43                 String[] datas = line.split("=");
44                 if (datas[0].equals(username) && datas[1].equals(password)) {
45                     flag = true;
46                     break;
47                 }
48             }
49         } catch (IOException e) {
50             e.printStackTrace();
51         } finally {
52             try {
53                 br.close();
54             } catch (IOException ex) {
55                 ex.printStackTrace();
56             }
57         }
58
59         return flag;
60     }
61
62     @Override
63     public void regist(User user) {
64         BufferedWriter bw = null;
65         try {
66             bw = new BufferedWriter(new FileWriter(file, true));
67             bw.write(user.getUsername() + "=" + user.getPassword());
68             bw.newLine();
69             bw.flush();
70         } catch (IOException e) {
71             e.printStackTrace();
72         } finally {
73             try {
74                 bw.close();
75             } catch (IOException ex) {
76                 ex.printStackTrace();
77             }
78         }
79     }
80 }
