#### SQLAlchemy对象的模型类定义
- 一对多关系模型的定义

  ps：表的构成：字段(字段类型 选项)、记录
  ~~~python
  from flask import Flask
  from flask_sqlalchemy import SQLAlchemy

  app = Flask(__name__)

  #设置连接数据库的URI
  app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:mysql@127.0.0.1:3306/test'	
  # 'mysql://[user]:[password]@[ip:port]/[database]'
  # 追踪对象的修改并且发送信号
  app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = True
  #查询时会显示原始SQL语句
  app.config['SQLALCHEMY_ECHO'] = True

  db = SQLAlchemy(app)

  class Role(db.Model):
      """定义角色模型类"""
      """角色--管理员、普通用户"""
      # 指定表名 ps:不指定表名，默认为类名小写
      __tablename__ = "roles"
      # 构造表的列对象，定义类属性
      id = db.Column(db.Integer, primary_key=True)
      name = db.Column(db.String(64), unique=True)
      # 额外添加属性
      users = db.relationship("User", backref="role")
      """
      添加额外属性，简化关联查询
      1、查询指定用户所属的角色
          user = User.query.get(1)
          Role.query.get(user.role_id)
          ==> user.role
      2、查询某角色下所有的用户
          role = Role.query.get(1)
          User.query.filter_by(role_id=role.id)
          ==> role.users
      """
      # repr()方法显示一个可读字符串
      def __repr__(self):
          return 'Role:%s' % self.name
      
  class User(db.Model):
      """定义用户模型类"""
      __tablename__ = "users"
      id = db.Column(db.Integer, primary_key=True)
      name = db.Column(db.String(64), unique=True, index=True)
      email = db.Column(db.String(64),unique=True)
      password = db.Column(db.String(64))
      # 外键关联
      role_id = db.Column(db.Integer, db.ForeignKey("roles.id"))
      # role_id = db.Column(db.Integer, db.ForeignKey(Role.id))
  	def __repr__(self):
      	return 'User:%s' % self.name
      
  @app.route("/")
  def index():
      return "index"

  if __name__ == "__main__":
      db.create_all()	# 项目中利用数据库的迁移创建表
      app.run(debug=True)
  ~~~

- 多对多关系模型定义

  ~~~python
  # 创建关系表 ps：此处student_id、course_id为字段名
  tb_student_course = db.Table('tb_student_course',
  				db.Column('student_id', db.Integer, db.ForeignKey('students.id')),
  				db.Column('course_id', db.Integer, db.ForeignKey('courses.id'))
                   )

  class Student(db.Model):
      __tablename__ = "students"
      id = db.Column(db.Integer, primary_key=True)
      name = db.Column(db.String(64), unique=True)

      courses = db.relationship('Course', 
                              secondary=tb_student_course,	# 指定多对多模型的关系表名
                              backref='student',	# 指定反向引用
                              lazy='dynamic')	# 指定动态加载，用到表数据时才去取数据
      
  class Course(db.Model):
      __tablename__ = "courses"
    	id = db.Column(db.Integer, primary_key=True)
    	name = db.Column(db.String(64), unique=True)
  ~~~

- 数据库的基本操作、数据库的迁移

    http://flask-sqlalchemy.pocoo.org/2.3/


  ​