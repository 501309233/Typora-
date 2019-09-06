



# 数据库

添加数据表qsx_promo

```sql

SET FOREIGN_KEY_CHECKS=0;

-- ----------------------------
-- Table structure for qsx_promo
-- ----------------------------
DROP TABLE IF EXISTS `qsx_promo`;
CREATE TABLE `qsx_promo` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `promo_name` varchar(255) COLLATE utf8_unicode_ci NOT NULL DEFAULT '' COMMENT '促销活动名称',
  `start_date` datetime NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '活动开始时间',
  `end_date` datetime NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '活动结束时间',
  `product_id` int(11) NOT NULL DEFAULT '0' COMMENT '产品id',
  `promo_product_price` double NOT NULL DEFAULT '0' COMMENT '促销价',
  `promo_product_num` int(11) NOT NULL DEFAULT '0' COMMENT '促销产品数量',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;

```



改变了引入商品列表的策略：

原版：直接引入商品列表

改版：查询活动列表获取有活动信息的商品

（不需要创建实体类PromoProductDTO，只需要在活动实体类PromoModel中）



2019年9月2日22点01分

做到了promoController这里，要把数据抛到前端接收渲染



2019年9月5日09点54分

秒杀前端页面数据传输和页面设计完毕