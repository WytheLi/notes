####django中模型类定义
- 一对多关系模型的定义
  ~~~python
  from django.db import models

  class BookInfo(models.Model):
      """定义图书模型类"""
      bname = models.CharField(max_length=20, verbose_name="书名")
      bpub_data = models.DateField(verbose_name="发布日期")
      bread = models.IntegerField(default=0, verbose_name="阅读量")
      bcomment = models.IntegerField(default=0, verbose_name="评论量")
      is_delete = models.BooleanField(default=False, verbose_name="逻辑删除")

      class Meta:
          db_table = "tb_books"	# 指明数据库表名
          verbose_name = "图书"	# 在admin站点中显示的名称
          verbose_name_plural = verbose_name	# 显示的复数名称

      def __str__(self):
          """定义对象显示信息"""
          return self.btitle

  class HeroInfo(models.Model):
      """定义英雄"""
      GENDER_CHOICES = (
          (0, 'male'),
          (1, 'female')
      )
      hname = models.CharField(max_length=20, verbose_name='名称') 
      hgender = models.SmallIntegerField(choices=GENDER_CHOICES, default=0, verbose_name='性别')  
      hcomment = models.CharField(max_length=200, null=True, verbose_name='描述信息') 
      hbook = models.ForeignKey(BookInfo, on_delete=models.CASCADE, verbose_name='图书')  # 外键
      is_delete = models.BooleanField(default=False, verbose_name='逻辑删除')

      class Meta:
          db_table = "tb_heros"
          verbose_name = "英雄"
          verbose_name_plural = verbose_name

      def __str__(self):
          return self.hname
  ~~~

- 项目中的模型类定义

  ~~~python
  class GoodsSpecification(BaseModel):
      """商品规格"""
      goods = models.ForeignKey(Goods, on_delete=models.CASCADE, verbose_name="商品")
      name = models.CharField(max_length=20, verbose_name="规格名称")

      class Meta:
          db_table = 'tb_goods_specification'
          verbose_name = '商品规格'
          verbose_name_plural = verbose_name

      def __str__(self):
          return "%s - %s" % (self.goods.name, self.name)


  class SpecificationOption(BaseModel):
      """规格选项"""
      spec = models.ForeignKey(GoodsSpecification, on_delete=models.CASCADE, verbose_name="规格")
      value = models.CharField(max_length=20, verbose_name="选项值")

      class Meta:
          db_table = 'tb_specification_option'
          verbose_name = '规格选项'
          verbose_name_plural = verbose_name

      def __str__(self):
          return '%s - %s' % (self.spec, self.value)


  class SKU(BaseModel):
      """SKU"""
      name = models.CharField(max_length=50, verbose_name="商品标题")
      caption = models.CharField(max_length=50, verbose_name="商品副标题")
      goods = models.ForeignKey(Goods, on_delete=models.CASCADE, verbose_name="商品")
      category = models.ForeignKey(GoodsCategory, on_delete=models.CASCADE, verbose_name="从属类别")
      price = models.DecimalField(max_digits=10, decimal_places=2, verbose_name="售价")
      cost_price = models.DecimalField(max_digits=10, decimal_places=2, verbose_name="进价")
      market_price = models.DecimalField(max_digits=10, decimal_places=2, verbose_name="市场价")
      comments = models.IntegerField(default=0, verbose_name="收藏数")
      sales = models.IntegerField(default=0, verbose_name="销售量")
      stock = models.IntegerField(default=0, verbose_name="库存")
      is_launched = models.BooleanField(default=True, verbose_name="是否上架")
      default_image_url = models.CharField(max_length=200, default="", null=True, blank=True, verbose_name="默认图片")

      class Meta:
          db_table = 'tb_sku'
          verbose_name = '商品SKU'
          verbose_name_plural = verbose_name

      def __str__(self):
          return '%s: %s' % (self.id, self.name)




  class SKUSpecification(BaseModel):
      """SKU具体规格"""
      sku = models.ForeignKey(SKU, on_delete=models.CASCADE, verbose_name="sku")
      spec = models.ForeignKey(GoodsSpecification, on_delete=models.CASCADE, verbose_name="规格")
      option = models.ForeignKey(SpecificationOption, on_delete=models.CASCADE, verbose_name="规格选项")

      class Meta:
          db_table = 'tb_sku_specification'
          verbose_name = 'SKU规格'
          verbose_name_plural = verbose_name

      def __str__(self):
          return '%s: %s - %s' % (self.sku, self.spec.name, self.option.value)
  ~~~

  ​