# Flask

## **Flask中向前端传输或者接收json数据的方法**

### 利用flask的request.form.get()方法

python端代码：


    @app.route('/sendjson', methods=['POST'])
    def sendjson():

    # 接受前端发来的数据
    data = json.loads(request.form.get('data'))

    # lesson: "Operation System"
    # score: 100
    lesson = data["lesson"]
    score = data["score"]

    # 自己在本地组装成Json格式,用到了flask的jsonify方法
    info = dict()
    info['name'] = "pengshuang"
    info['lesson'] = lesson
    info['score'] = score
    return jsonify(info)

Javascript代码：

    <script>
        $(document).ready(function () {
        var data = {
            data: JSON.stringify({"lesson": "Operation System", "score": 100})
    }
        $.ajax({
            url:"/sendjson",
            type: 'POST',
            data: data,
            success: function (msg) {
                alert(msg.name)
            }
        })
    });
    </script>
        
    </script>
    
### 利用flask的request.get_data()方法

python端代码：

    @app.route('/sendjson2',methods=['POST'])
    def sendjson2():

    # 接收前端发来的数据,转化为Json格式,我个人理解就是Python里面的字典格式
    data = json.loads(request.get_data())

    # 然后在本地对数据进行处理,再返回给前端
    name = data["name"]
    age = data["age"]
    location = data["location"]
    data["time"] = "2016"

    # Output: {u'age': 23, u'name': u'Peng Shuang', u'location': u'China'}
    # print data
    return jsonify(data)

Javascript代码：

    <script>
        $(document).ready(function () {
            var student = new Object();
            student.name = "Peng Shuang";
            student.age = 23;
            student.location = "China";
            var data = JSON.stringify(student)

        $.ajax({
            url: "/sendjson2",
            type: "POST",
            data: data,
            success: function (msg) {
                alert(msg.time)
            }
        })
        })
    </script>
    


## **前端接收flask后端的返回值**

前端接收flask返回主要有两种方法，一种是直接在html页面中嵌套语句，一种是利用js解析，后者更具通用性和可修改性。  

**第一种：**    

这种方法，后端的返回值可以是字典类型、数组或者单变量，但是不能是json格式，解析不到。data是返回的参数名，与要与前端html对应，名字要相同，前端可以直接取值。  

前端代码：  

    <!DOCTYPE html>
    <html lang="zh-CN">
        <head>
            <meta charset="UTF-8">
        </head>
    <table border="1"  style="margin:0 auto; background-color:#5abef8;">
        <tr>
            <th>名字</th>
            <th>编号</th> 
        </tr>
    {% for i in data %}
        <tr>
            <td>{{ i.name }}<br>{{ i.name }}</td>
            <td>{{ i.num }}<br>{{ i.num }}</td>
        </tr>
    {% endfor %}
    </table>
    </body>
    </html>

后端代码：  

    @app.route("/")
    def index():
        dic={}
        dic['name']='xiaomin'
        dic['num']=1
        return render_template('index.html',data=dic)


