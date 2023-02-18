---
id: arch-database-design
title: Database design
sidebar_label: Database design
---
import Mermaid from '@theme/Mermaid'

```sql
CREATE DATABASE mpbackend default character set utf8;

USE mpbackend;

CREATE TABLE `mpbackend_account_login_method` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT 'id',
  `user_id` bigint NOT NULL COMMENT 'user id relation',
  `provider_key` varchar(320) DEFAULT NULL COMMENT 'provider_key is user_id from provider',
  `login_provider` enum('binance-oauth','okta','email') NOT NULL COMMENT 'loginProvider can be okta/oauth/binance',
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'field created time',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'field updated time',
  `db_create_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) COMMENT 'database insertion time, please do not modify',
  `db_modify_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) ON UPDATE CURRENT_TIMESTAMP(3) COMMENT 'database update time, please do not modify',
  `disabled_at` timestamp NULL DEFAULT NULL COMMENT 'disabled time',
  `disabled` tinyint(1) DEFAULT '0' COMMENT 'soft delete',
  PRIMARY KEY (`id`),
  KEY `idx_user_id` (`user_id`),
  KEY `idx_provider_key` (`provider_key`)
) ENGINE=InnoDB AUTO_INCREMENT=40 DEFAULT CHARSET=utf8 COMMENT='user login method to the service';


CREATE TABLE `mpbackend_admin_user_role_binding` (
  `id` bigint NOT NULL AUTO_INCREMENT,
  `user_id` bigint NOT NULL,
  `role_id` bigint NOT NULL,
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'field created time',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'field updated time',
  `db_create_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) COMMENT 'database insertion time, please do not modify',
  `db_modify_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) ON UPDATE CURRENT_TIMESTAMP(3) COMMENT 'database update time, please do not modify',
  `disabled_at` timestamp NULL DEFAULT NULL COMMENT 'disabled time',
  `disabled` tinyint(1) DEFAULT '0' COMMENT 'soft delete',
  PRIMARY KEY (`id`),
  KEY `idx_user_id` (`user_id`),
  KEY `idx_role_id` (`role_id`)
) ENGINE=InnoDB AUTO_INCREMENT=21 DEFAULT CHARSET=utf8 COMMENT='admin user role';


CREATE TABLE `mpbackend_application` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT 'id',
  `app_id` varchar(255) NOT NULL COMMENT 'unique appId',
  `name` varchar(30) DEFAULT NULL COMMENT 'application name',
  `description` text COMMENT 'description of application',
  `avatar_url` varchar(255) DEFAULT NULL COMMENT 'avatar image url',
  `is_active` tinyint(1) DEFAULT '1' COMMENT 'is active',
  `current_published_version_id` bigint DEFAULT NULL COMMENT 'current published version id relation',
  `current_auditing_version_id` bigint DEFAULT NULL COMMENT 'current auditing version id relation',
  `current_experience_version_id` bigint DEFAULT NULL COMMENT 'current experience version id relation',
  `qrcode` text COMMENT 'qrcode json object of online, experience and auditing',
  `developer_id` bigint NOT NULL COMMENT 'developer relation id',
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'field created time',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'field updated time',
  `db_create_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) COMMENT 'database insertion time, please do not modify',
  `db_modify_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) ON UPDATE CURRENT_TIMESTAMP(3) COMMENT 'database update time, please do not modify',
  `disabled_at` timestamp NULL DEFAULT NULL COMMENT 'disabled time',
  `disabled` tinyint(1) DEFAULT '0' COMMENT 'soft delete',
  `min_android_version` varchar(64) DEFAULT NULL COMMENT 'min android version for the application',
  `min_ios_version` varchar(64) DEFAULT NULL COMMENT 'min ios version for the application',
  `development_settings` text COMMENT 'application development settings json',
  `web_app_url` varchar(256) DEFAULT NULL COMMENT 'web url for application',
  `app_type` enum('mini-program','web-app') DEFAULT NULL COMMENT 'type of application',
  PRIMARY KEY (`id`),
  UNIQUE KEY `idx_app_id` (`app_id`) COMMENT 'app relation id',
  KEY `idx_current_published_version_id` (`current_published_version_id`),
  KEY `idx_current_auditing_version_id` (`current_auditing_version_id`),
  KEY `idx_current_experience_version_id` (`current_experience_version_id`),
  KEY `idx_developer_id` (`developer_id`)
) ENGINE=InnoDB AUTO_INCREMENT=32 DEFAULT CHARSET=utf8 COMMENT='application table';


CREATE TABLE `mpbackend_application_permission_flag` (
  `id` bigint NOT NULL AUTO_INCREMENT,
  `name` varchar(256) NOT NULL COMMENT 'name of the permission flag',
  `arguments` text COMMENT 'string array arguments',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='application permission flag table';


CREATE TABLE `mpbackend_application_permission_flag_binding` (
  `id` bigint NOT NULL AUTO_INCREMENT,
  `application_id` varchar(255) NOT NULL COMMENT 'application relation id',
  `permission_flag_id` bigint NOT NULL COMMENT 'permission flag id relation',
  PRIMARY KEY (`id`),
  KEY `idx_application_id` (`application_id`),
  KEY `idx_permission_flag_id` (`permission_flag_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='application permisison flag binding table';


CREATE TABLE `mpbackend_application_user_role_binding` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT 'id',
  `user_id` bigint NOT NULL,
  `role_id` bigint NOT NULL,
  `application_id` varchar(255) NOT NULL,
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'field created time',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'field updated time',
  `db_create_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) COMMENT 'database insertion time, please do not modify',
  `db_modify_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) ON UPDATE CURRENT_TIMESTAMP(3) COMMENT 'database update time, please do not modify',
  `disabled_at` timestamp NULL DEFAULT NULL COMMENT 'disabled time',
  `disabled` tinyint(1) DEFAULT '0' COMMENT 'soft delete',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uniq_idx_application_user_role` (`application_id`,`user_id`,`role_id`),
  KEY `idx_role_id` (`role_id`),
  KEY `idx_application_id` (`application_id`)
) ENGINE=InnoDB AUTO_INCREMENT=89 DEFAULT CHARSET=utf8 COMMENT='application version table';


CREATE TABLE `mpbackend_application_version` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT 'id',
  `version` varchar(256) DEFAULT NULL COMMENT 'semantic version',
  `description` text COMMENT 'description of the application version',
  `uploader_id` bigint NOT NULL COMMENT 'uploader id',
  `is_experience` tinyint(1) DEFAULT '0' COMMENT 'experience flag',
  `revision` varchar(64) NOT NULL COMMENT 'md5 hash of the file',
  `download_key` varchar(256) NOT NULL COMMENT 'download_key for the application version',
  `download_url` varchar(256) DEFAULT NULL COMMENT 'download url for the application version',
  `sdk_version` varchar(64) NOT NULL COMMENT 'minumum sdk version',
  `status` enum('PREVIEW','DEVELOP','AUDITING','APPROVED','REJECTED','PUBLISHED') DEFAULT 'DEVELOP' COMMENT 'status of the application version',
  `published_timestamp` timestamp NULL DEFAULT NULL COMMENT 'time when the application version published',
  `application_id` varchar(255) NOT NULL COMMENT 'application relation id',
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'field created time',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'field updated time',
  `db_create_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) COMMENT 'database insertion time, please do not modify',
  `db_modify_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) ON UPDATE CURRENT_TIMESTAMP(3) COMMENT 'database update time, please do not modify',
  `disabled_at` timestamp NULL DEFAULT NULL COMMENT 'disabled time',
  `disabled` tinyint(1) DEFAULT '0' COMMENT 'soft delete',
  PRIMARY KEY (`id`),
  KEY `idx_uploader_id` (`uploader_id`),
  KEY `idx_application_id` (`application_id`),
  KEY `idx_version` (`version`)
) ENGINE=InnoDB AUTO_INCREMENT=1231 DEFAULT CHARSET=utf8 COMMENT='application version table';


CREATE TABLE `mpbackend_application_version_audit` (
  `id` bigint NOT NULL AUTO_INCREMENT,
  `remark` text,
  `is_approved` tinyint(1) DEFAULT NULL,
  `reviewer_id` bigint DEFAULT NULL,
  `application_version_id` bigint NOT NULL,
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'field created time',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'field updated time',
  `db_create_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) COMMENT 'database insertion time, please do not modify',
  `db_modify_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) ON UPDATE CURRENT_TIMESTAMP(3) COMMENT 'database update time, please do not modify',
  `disabled_at` timestamp NULL DEFAULT NULL COMMENT 'disabled time',
  `disabled` tinyint(1) DEFAULT '0' COMMENT 'soft delete',
  PRIMARY KEY (`id`),
  KEY `idx_reviewer_id` (`reviewer_id`),
  KEY `idx_application_version_id` (`application_version_id`)
) ENGINE=InnoDB AUTO_INCREMENT=228 DEFAULT CHARSET=utf8 COMMENT='application version audit table';


CREATE TABLE `mpbackend_developer` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT 'id',
  `name` varchar(255) DEFAULT NULL COMMENT 'name of developer',
  `settings` text COMMENT 'settings for developer',
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'field created time',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'field updated time',
  `db_create_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) COMMENT 'database insertion time, please do not modify',
  `db_modify_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) ON UPDATE CURRENT_TIMESTAMP(3) COMMENT 'database update time, please do not modify',
  `disabled_at` timestamp NULL DEFAULT NULL COMMENT 'disabled time',
  `disabled` tinyint(1) DEFAULT '0' COMMENT 'soft delete',
  `owner_id` bigint NOT NULL COMMENT 'owner user id relation',
  PRIMARY KEY (`id`),
  KEY `idx_owner_id` (`owner_id`)
) ENGINE=InnoDB AUTO_INCREMENT=15 DEFAULT CHARSET=utf8 COMMENT='developer account table';


CREATE TABLE `mpbackend_developer_user` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT 'id',
  `developer_id` bigint NOT NULL COMMENT 'developer relation id',
  `user_id` bigint NOT NULL COMMENT 'user relation id',
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'field created time',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'field updated time',
  `db_create_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) COMMENT 'database insertion time, please do not modify',
  `db_modify_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) ON UPDATE CURRENT_TIMESTAMP(3) COMMENT 'database update time, please do not modify',
  `disabled_at` timestamp NULL DEFAULT NULL COMMENT 'disabled time',
  `disabled` tinyint(1) DEFAULT '0' COMMENT 'soft delete',
  PRIMARY KEY (`id`),
  KEY `idx_user_id` (`user_id`),
  KEY `idx_developer_id` (`developer_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='developer user table';


CREATE TABLE `mpbackend_developer_user_role_binding` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT 'id',
  `user_id` bigint NOT NULL COMMENT 'user relation id',
  `role_id` bigint NOT NULL COMMENT 'role relation id',
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'field created time',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'field updated time',
  `db_create_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) COMMENT 'database insertion time, please do not modify',
  `db_modify_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) ON UPDATE CURRENT_TIMESTAMP(3) COMMENT 'database update time, please do not modify',
  `disabled_at` timestamp NULL DEFAULT NULL COMMENT 'disabled time',
  `disabled` tinyint(1) DEFAULT '0' COMMENT 'soft delete',
  `developer_id` bigint NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `idx_user_role_developer` (`user_id`,`role_id`,`developer_id`) COMMENT 'user role developer index',
  KEY `idx_user_id` (`user_id`),
  KEY `idx_role_id` (`role_id`),
  KEY `idx_developer_id` (`developer_id`)
) ENGINE=InnoDB AUTO_INCREMENT=63 DEFAULT CHARSET=utf8 COMMENT='developer user role';


CREATE TABLE `mpbackend_file` (
  `id` bigint NOT NULL AUTO_INCREMENT,
  `file_key` varchar(256) NOT NULL,
  `uploader_id` bigint NOT NULL,
  `type` enum('APPLICATION','IMAGE') NOT NULL COMMENT 'type of the file',
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'field created time',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'field updated time',
  `db_create_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) COMMENT 'database insertion time, please do not modify',
  `db_modify_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) ON UPDATE CURRENT_TIMESTAMP(3) COMMENT 'database update time, please do not modify',
  `disabled_at` timestamp NULL DEFAULT NULL COMMENT 'disabled time',
  `disabled` tinyint(1) DEFAULT '0' COMMENT 'soft delete',
  PRIMARY KEY (`id`),
  UNIQUE KEY `idx_file_key` (`file_key`) COMMENT 'unique file key index',
  KEY `idx_uploader_id` (`uploader_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1998 DEFAULT CHARSET=utf8 COMMENT='file table';


CREATE TABLE `mpbackend_role` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT 'id',
  `name` varchar(16) DEFAULT NULL COMMENT 'name of role',
  `permissions` varchar(255) DEFAULT NULL COMMENT 'permissions that are separated by comma',
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'field created time',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'field updated time',
  `db_create_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) COMMENT 'database insertion time, please do not modify',
  `db_modify_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) ON UPDATE CURRENT_TIMESTAMP(3) COMMENT 'database update time, please do not modify',
  `disabled_at` timestamp NULL DEFAULT NULL COMMENT 'disabled time',
  `disabled` tinyint(1) DEFAULT '0' COMMENT 'soft delete',
  `scope` enum('Application','Developer','Admin') DEFAULT NULL COMMENT 'enum scope of the role',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=15 DEFAULT CHARSET=utf8 COMMENT='roles definition and permissions';


CREATE TABLE `mpbackend_user` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT 'id',
  `email` varchar(320) NOT NULL COMMENT 'user email',
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'field created time',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'field updated time',
  `db_create_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) COMMENT 'database insertion time, please do not modify',
  `db_modify_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) ON UPDATE CURRENT_TIMESTAMP(3) COMMENT 'database update time, please do not modify',
  `disabled_at` timestamp NULL DEFAULT NULL COMMENT 'disabled time',
  `disabled` tinyint(1) DEFAULT '0' COMMENT 'soft delete',
  PRIMARY KEY (`id`),
  KEY `idx_email` (`email`)
) ENGINE=InnoDB AUTO_INCREMENT=59 DEFAULT CHARSET=utf8 COMMENT='base user table';


CREATE TABLE `mpbackend_user_session_token` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT 'id',
  `session_token` varchar(256) NOT NULL COMMENT 'sesion token',
  `access_token` varchar(1024) DEFAULT NULL COMMENT 'access token returned by login provider',
  `refresh_token` varchar(1024) DEFAULT NULL COMMENT 'refresh token returned by login provider',
  `user_id` bigint NOT NULL COMMENT 'user id relation',
  `expired_at` timestamp NOT NULL COMMENT 'session expired time',
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'field created time',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT 'field updated time',
  `db_create_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) COMMENT 'database insertion time, please do not modify',
  `db_modify_time` datetime(3) DEFAULT CURRENT_TIMESTAMP(3) ON UPDATE CURRENT_TIMESTAMP(3) COMMENT 'database update time, please do not modify',
  `disabled_at` timestamp NULL DEFAULT NULL COMMENT 'disabled time',
  `disabled` tinyint(1) DEFAULT '0' COMMENT 'soft delete',
  PRIMARY KEY (`id`),
  UNIQUE KEY `idx_session_token` (`session_token`) COMMENT 'session token unique index',
  KEY `idx_user_id` (`user_id`)
) ENGINE=InnoDB AUTO_INCREMENT=482 DEFAULT CHARSET=utf8 COMMENT='user session table to keep track session';

```
