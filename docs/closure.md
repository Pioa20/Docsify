# Flask

## Flask中向前端传输或者接收json数据的方法

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
    




