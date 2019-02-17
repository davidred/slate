# Users

## Get a Specific User

## Get a Specific Property

```http
GET /api/users/<id> HTTP/1.1
Accept: application/json
Host: roomeze.com
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "data": {
    "id": 82,
    "type": "user",
    "attributes": {
      "role": "user",
      "questionnaire_comlete": true,
      "showing_request_questionnaire_complete": true,
      "created_at": "2017-01-29T21:59:30.064-05:00",
      "updated_at": "2019-02-13T02:55:58.659-05:00",
      "first_name": "Jimbob",
      "last_initial": "C"
    },
    "relationships": {
      "user_profile": {
        "data": [
          {
            "id": "82",
            "type": "user_profile"
          }
        ]
      },
      "images": {
        "data": [
          {
            "id": "24",
            "type": "image"
          }
        ]
      },
      "neighborhoods": {
        "data": [
          {
            "id": "222",
            "type": "neighborhood"
          }
        ]
      },
      "cities": {
        "data": [
          {
            "id": "2",
            "type": "city"
          }
        ]
      }
    }
  }
}
```

This endpoint retrieves a specific user.

### HTTP Request

`GET https://roomeze.com/api/users/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID        | The ID of the user to fetch

### Authentication

By default, only users with public profiles are returned. To filter users with out public profiles, you must be signed in as an admin.

### Including Related Resources

`GET https://roomeze.com/api/users/2?include[]=user_profile&include[]=images`

Parameter     | Description
------------- | -----------
user_profile  | The user profile belonging to the user
images        | The users images
neighborhoods | The neighborhood preferences selected by the user
cities        | The city preferences selected by the user

## Get All Users

```http
GET /api/users HTTP/1.1
Accept: application/json
Host: roomeze.com
```
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "meta": {
    "page_info": {
      "total_pages": 5,
      "current_page": 1,
      "prev_page": null,
      "next_page": 2,
      "page_size": 25,
      "total_count": 115
    }
  },
  "data": [
    {
      "id": "2",
      "type": "user",
      "attributes": {
        "role": "admin",
        "questionnaire_complete": true,
        "showing_request_questionnaire_complete": true,
        "created_at": "2017-01-29T21:59:30.064-05:00",
        "updated_at": "2019-02-13T02:55:58.659-05:00",
        "first_name": "Jimbob",
        "last_initial": "C"
      }
    },
    {
      "id": "3",
      "type": "user",
      "attributes": {
        "role": "user",
        "questionnaire_complete": true,
        "showing_request_questionnaire_complete": true,
        "created_at": "2017-01-29T21:59:30.064-05:00",
        "updated_at": "2019-02-13T02:55:58.659-05:00",
        "first_name": "David",
        "last_initial": "R"
      }
    }
  ]
}
```

This endpoint retrieves all users.

### HTTP Request

`GET https://roomeze.com/api/users`

### Authentication

By default, only users with public profiles are returned. To filter users with out public profiles, you must be signed in as an admin.

### Including Related Resources

See Get User section for a list of possible resources

### Filter Parameters

`GET https://roomeze.com/api/users?filter[age_min]=21`

Parameter          | Type    | Example                            | Description
------------------ | ------- | ---------------------------------- | -----------
public_profile     | Boolean | ?filter[public_profile]=false      | Whether or not the user has set their profile to private (Admin only)
age_min            | Integer | ?filter[age_min]=21                | Users older than the specified age.
age_max            | Integer | ?filter[age_max]=25                | Users younger than the specified age.
move_in_period     | String  | ?filter[move_in_period]=browsing   | Users with the specified move_in_period. Possible options are: <code>month</code>, <code>year</code>, <code>browsing</code>.
gender             | String  | ?filter[gender]=female             | Users with the specified gender. Possible options are: <code>male</code>, <code>female</code>
role               | String  | ?filter[role]=matchmaker           | Users with the specified role. Possible options are: <code>user</code>, <code>matchmaker</code>, <code>admin</code>, <code>client_admin</code>

## Create a User

```http
POST /api/users HTTP/1.1
Accept: application/json
Authorization: Token tOQ4mkzlsJJYX1CWoxvuNPOdeNJjZGOTJ
Host: roomeze.com

{
  "data": {
    "type": "user",
    "attributes": {
      "email": "david@example.com",
      "first_name": "JimBob",
      "last_name": "Cooter"
    },
    "relationships": {
      "user_profile": {
        "data": {
          "attributes": {
            "gender": "male",
            "date_of_birth": "1980-12-20"
          }
        }
      },
      "cities": [
        {
          "type": "city",
          "id": "1"
        },
        {
          "type": "city",
          "id": "3"
        }
      ]
    }
  }
}
```
```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: https://roomeze.com/api/users/2

{
  "data": {
    "id": 2,
    "type": "user",
    "attributes": {
      "first_name": "JimBob",
      "last_initial": "C",
      "age": 39,
      "role": "user",
      "questionnaire_complete": false,
      "showing_request_questionnaire_complete": false
    },
    "relationships": {
      "user_profile": {
        "data": {
          "id": 3,
          "type": "user_profile"
        }
      },
      "cities": [
        {
          "type": "city",
          "id": "1"
        },
        {
          "type": "city",
          "id": "3"
        }
      ]
    }
  }
}
```

This endpoint creates a user

### HTTP Request

`POST https://roomeze.com/api/users`

### Attributes

Parameter  | Type   | Description
---------- | ------ | -----------
first_name | String | The first name of the user
last_name  | String | The last name of the user
email      | String | The email address of the user
phone      | String | The phone number of the user

### Related Resources

Users can be created with associated user profile parameters provided. These can be added the the `user_profile` object under `relationships`. Valid user profile parameters are described below.

#### User Profiles

Parameter               | Type    | Description
----------------------- | ------- | -----------
gender                  | String  | The gender of the user. Possible options are: `male`, `female`
date_of_birth           | String  | The date of birth of the user. Format: `YYYY-MM-DD`
move_in_date            | String  | The date the user is planning to move in. Format `YYYY-MM-DD`
budget                  | Integer | The estimated monthly budget for an apartment
has_cat                 | Boolean | Whether or not the user has a cat
has_dog                 | Boolean | Whether or not the user has a dog
no_cats                 | Boolean | Whether or not the user can live with a cat
no_dogs                 | Boolean | Whether or not the user can live with a dog
can_sign_one_year_lease | Boolean | Whether or not the user can sign a one year lease
applying_with_guarantor | Boolean | Whether or not the user is applying with a guarantor
income                  | Integer | The income of the user

#### Cities

Parameter | Type    | Description
--------- | ------- | -----------
type      | String  | The type of related resource. The value should be `city`
id        | Integer | The id of the related city

## Update a User

```http
PATCH /api/users/2 HTTP/1.1
Accept: application/json
Authorization: Token tOQ4mkzlsJJYX1CWoxvuNPOdeNJjZGOTJ
Host: roomeze.com

{
  "data": {
    "id": 2,
    "type": "user",
    "attributes": {
      "email": "david@example.com",
      "first_name": "JimBob",
      "last_name": "Cooter",
      "phone": "+1 212 555 6789"
    },
    "relationships": {
      "user_profile": {
        "type": "user_profile",
        "data": {
          "attributes": {
            "gender": "male",
            "date_of_birth": "1980-12-20"
          }
        }
      },
      "cities": [
        {
          "type": "city",
          "id": "1"
        },
        {
          "type": "city",
          "id": "3"
        }
      ]
    }
  },
}
```
```http
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://roomeze.com/api/users/2

{
  "data": {
    "id": 2,
    "type": "user",
    "attributes": {
      "first_name": "JimBob",
      "last_initial": "C",
      "age": 39,
      "role": "user",
      "questionnaire_complete": false,
      "showing_request_questionnaire_complete": false
    },
    "relationships": {
      "user_profile": {
        "data": {
          "id": 3,
          "type": "user_profile"
        }
      },
      "cities": [
        {
          "type": "city",
          "id": "1"
        },
        {
          "type": "city",
          "id": "3"
        }
      ]
    }
  }
}
```

This endpoint updates a user

### HTTP Request

`PATCH https://roomeze.com/api/users/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID        | The ID of the user to update

### Parameters

Parameter  | Type   | Description
---------- | ------ | -----------
first_name | String | The first name of the user
last_name  | String | The last name of the user
email      | String | The email address of the user
phone      | String | The phone number of the user

### Related Resources

Users can be created with associated user profile parameters provided. These can be added the the `user_profile` object under `relationships`. Valid user profile parameters are described below.

#### User Profiles

Parameter               | Type    | Description
----------------------- | ------- | -----------
gender                  | String  | The gender of the user. Possible options are: `male`, `female`
date_of_birth           | String  | The date of birth of the user. Format: `YYYY-MM-DD`
move_in_date            | String  | The date the user is planning to move in. Format `YYYY-MM-DD`
budget                  | Integer | The estimated monthly budget for an apartment
has_cat                 | Boolean | Whether or not the user has a cat
has_dog                 | Boolean | Whether or not the user has a dog
no_cats                 | Boolean | Whether or not the user can live with a cat
no_dogs                 | Boolean | Whether or not the user can live with a dog
can_sign_one_year_lease | Boolean | Whether or not the user can sign a one year lease
applying_with_guarantor | Boolean | Whether or not the user is applying with a guarantor
income                  | Integer | The income of the user

#### Cities

Parameter | Type    | Description
--------- | ------- | -----------
type      | String  | The type of related resource. The value should be `city`
id        | Integer | The id of the related city