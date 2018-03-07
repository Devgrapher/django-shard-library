
# Django-Shard-Library

## Introduction
- 장고 ORM에서 샤드기능을 지원하기 위한 라이브러리 입니다.

## Requirement
- `Django 2.0.0` or higher
- `Python 3.6` or higher

## Usage
#### Setup
``` python
DATABASES = ConfigHelper.database_configs(
    unshard = {
        'default': {
            'master': 'mysql://user:pwd@host/metadata',
            'slaves': ['mysql://user:pwd@host/metadata', ]
        },
    },
    shard = {
        'SHARD_NAME': {
            'options': {
                'database_name': 'product',
                'logical_count': 4,
            },
            'shards': [
                {
                    'master': 'mysql://user:pwd@host/',
                    'slaves': ['mysql://user:pwd@host/', 'mysql://user:pwd@host/',]
                },
                {
                    'master': 'mysql://user:pwd@host/',
                    'slaves': ['mysql://user:pwd@host/', 'mysql://user:pwd@host/',]
                }
            ],
        },
    },
)

```

#### Model
``` python
class ShardModel(ShardMixin, models.Model):
    user_id = models.IntegerField(null=False, verbose_name='User Idx')

    shard_group = 'SHARD_NAME'
    shard_key_name = 'user_id'
```

- Shard Key는 현재 Integer만 지원합니다.

## Dependency
- [dj-database-url](https://github.com/kennethreitz/dj-database-url)

## TODO

- 모든 샤드에 공유되는 Static데이터기능 구현
- migration
