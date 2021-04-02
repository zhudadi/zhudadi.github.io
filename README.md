<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>测试</title>
    <style>
        h1{
            text-align: center;
            display: block;
            margin-bottom: 0;
            color: #4caf50;
        }
        #customers{
            margin: 0px auto;
            font-family: Arial, Helvetica, sans-serif;
            border-collapse: collapse;
        }
        #customers th, #customers td{
            border: 1px #ddd solid;
            padding: 8px;
        }
        #customers tr:nth-child(even){
            background-color: #f2f2f2;
        }
        #customers tr:hover{
            background-color: #ddd;
        }
        #customers th{
            background-color: #4caf50;
            color: white;
            padding: 12px auto;
            text-align: left;
        }
    </style>
</head>
<body>
    <h1>Table</h1>
    <table id="customers">
        <tr>
            <th>Company</th>
            <th>Contact</th>
            <th>Address</th>
            <th>City</th>
        </tr>
        <tr>
            <td>Alibaba</td>
            <td>Ma Yun</td>
            <td>No. 699, Wangshang Road, Binjiang District</td>
            <td>Hangzhou</td>
        </tr>
        <tr>
            <td>APPLE</td>
            <td>Tim Cook</td>
            <td>1 Infinite Loop Cupertino, CA 95014</td>
            <td>Cupertino</td>
        </tr>
        <tr>
            <td>BAIDU</td>
            <td>Li YanHong</td>
            <td>Lixiang guoji dasha,No 58, beisihuanxilu</td>
            <td>Beijing</td>
        </tr>
        <tr>
            <td>Canon</td>
            <td>Tsuneji Uchida</td>
            <td>One Canon Plaza Lake Success, NY 11042</td>
            <td>New York</td>
        </tr>
        <tr>
            <td>Google</td>
            <td>Larry Page</td>
            <td>1600 Amphitheatre Parkway Mountain View, CA 94043</td>
            <td>Mountain View</td>
        </tr>
        <tr>
            <td>HUAWEI</td>
            <td>Ren Zhengfei</td>
            <td>Putian Huawei Base, Longgang District</td>
            <td>Shenzhen</td>
        </tr>
        <tr>
            <td>Microsoft</td>
            <td>Bill Gates</td>
            <td>15700 NE 39th St Redmond, WA 98052</td>
            <td>Redmond</td>
        </tr>
        <tr>
            <td>Nokia</td>
            <td>Olli-Pekka Kallasvuo</td>
            <td>P.O. Box 226, FIN-00045 Nokia Group</td>
            <td>Helsinki</td>
        </tr>
        <tr>
            <td>SONY</td>
            <td>Kazuo Hirai</td>
            <td>Park Ridge, NJ 07656</td>
            <td>Park Ridge</td>
        </tr>
        <tr>
            <td>Tencent</td>
            <td>Ma Huateng</td>
            <td>Tencent Building, High-tech Park, Nanshan District</td>
            <td>Shenzhen</td>
        </tr>
    </table>
</body>
</html>
