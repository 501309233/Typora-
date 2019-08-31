



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

引入促销活动模块

创建实体类PromoProductDTO extends ProductDTO

引入促销活动聚合模型