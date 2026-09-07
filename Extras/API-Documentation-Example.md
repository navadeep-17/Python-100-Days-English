## API Documentation Reference Example

0. User Login - **POST** `/api/login/`

      Developer: Luo Hao

      Version: v1

      Last Modified:

      Description: After successful login, a user token will be generated or updated.

      Usage: The test database has four pre-configured accounts available for use, as shown in the table below.

      | Username   | Password | Role                  |
      | ---------- | -------- | --------------------- |
      | jackfrued  | 123456   | Administrator         |
      | wangdachui | 123123   | Regular User          |
      | hellokitty | 123123   | Real Estate Agent     |
      | wuzetian   | 123456   | Landlord              |

      Request Parameters:

      | Parameter | Type   | Required | Location     | Description |
      | --------- | ------ | -------- | ------------ | ----------- |
      | username  | String | Yes      | Request Body | Username    |
      | password  | String | Yes      | Request Body | Password    |

      Response:

      - Login successful:

        ```JSON
        {
            "code": 30000,
            "message": "User login successful",
            "token": "f83e0f624e2311e9af1f00163e02b646"
        }
        ```

      - Login failed:

        ```JSON
        {
            "code": 30001,
            "message": "Incorrect username or password"
        }
        ```

1. Send SMS Verification Code - **GET** `/api/mobile_code/{domestic_phone_number}/`

   Developer: Luo Hao

   Version: v1

   Description: API for sending an SMS verification code to a specified phone number. The phone number must be a domestic (Chinese) phone number and should be included as a path parameter in the URL. When the API indicates the SMS was sent successfully, the specified phone number will not actually receive the SMS, because the test SMS credits from the third-party SMS platform have been used up.

   Usage: International country codes are not currently supported for domestic phone numbers.

   Request Parameters: None.

   Response:

   - Request successful:

     ```JSON
     {
         "code": 10001,
         "msg": "SMS verification code sent successfully"
     }
     ```

   - Less than 60 seconds between two requests:

     ```JSON
     {
         "code": 10002,
         "msg": "Please do not resend the verification code within 60 seconds"
     }
     ```

   - Invalid phone number:

     ```JSON
     {
         "code": 10003,
         "msg": "Please provide a valid phone number"
     }
     ```

   - SMS service platform failure:

     ```JSON
     {
         "code": 10004,
         "msg": "SMS service is temporarily unavailable"
     }
     ```

2. Get All Provincial Administrative Units - **GET** `/api/districts/`

   Developer: Luo Hao

   Version: v1

   Description: None.

   Usage: None.

   Request Parameters: None.

   Response:

   ```JSON
   [
       {
           "distid": 110000,
           "name": "Beijing"
       },
       {
           "distid": 120000,
           "name": "Tianjin"
       }
   ]
   ```

3. Get Details of a Specified Administrative Unit and Its Subordinate Units - **GET** `/api/districts/{district_id}/`

   Developer: Luo Hao

   Version: v1

   Description: Specify an administrative unit ID via the URL parameter. If the ID is for a provincial-level unit, it returns the province and its subordinate city-level units. If the ID is for a city-level unit, it returns the city and its subordinate district/county-level units. If the ID is for a district/county-level unit, it returns that unit's information with the `cities` property set to `[]`.

   Usage: In the database, except for Sichuan Province, the "intro" data for other administrative units has not been entered, and this field may be an empty string.

   Request Parameters: None.

   Response:

   ```JSON
   {
       "distid": 510000,
       "name": "Sichuan Province",
       "intro": "Located in the inland southwest of China, bordering Chongqing to the east, Yunnan and Guizhou to the south, Tibet to the west, and Shaanxi, Gansu, and Qinghai to the north. Sichuan Province has a total area of 486,000 square kilometers, with its capital in Chengdu. As of the end of 2018, Sichuan Province has 18 prefecture-level cities, 3 autonomous prefectures, 17 county-level cities, 108 counties, 4 autonomous counties, and 54 municipal districts.",
       "cities": [
           {
               "distid": 510100,
               "name": "Chengdu"
           },
           {
               "distid": 510300,
               "name": "Zigong"
           },
           {
               "distid": 510400,
               "name": "Panzhihua"
           }
       ]
   }
   ```

4. Get Popular Cities - **GET** `/api/hotcities/`

   Developer: Luo Hao

   Version: v1

   Description: None.

   Usage: None.

   Request Parameters: None.

   Response:

   ```JSON
   [
       {
           "distid": 110100,
           "name": "Beijing"
       },
       {
           "distid": 120100,
           "name": "Tianjin"
       },
       {
           "distid": 130100,
           "name": "Shijiazhuang"
       }
   ]
   ```

5. Get Real Estate Agents with Pagination - **GET** `/api/agents/`

   Developer: Luo Hao

   Version: v1

   Description: Agent names are matched using prefix fuzzy matching. Agent service star rating means the agent's service rating must be no lower than the specified rating. Agent certification status has only two options: 0 (not certified) and 1 (certified). The filter conditions represented by the three parameters are combined using AND logic. The result is paginated real estate agent information.

   Usage: None.

   Request Parameters:

   | Parameter | Type   | Required | Location        | Description                                 |
   | --------- | ------ | -------- | --------------- | ------------------------------------------- |
   | name      | String | No       | Query Parameter | Agent name                                  |
   | key       | String | No       | Query Parameter | Agent service star rating                   |
   | cert      | String | No       | Query Parameter | Whether agent is certified                  |
   | page      | Integer| No       | Query Parameter | Page number, default 1                      |
   | size      | Integer| No       | Query Parameter | Page size, default 5, maximum 50            |

   Response:

   ```JSON
   {
       "count": 1,
       "next": null,
       "previous": null,
       "results": [
           {
               "agentid": 6,
               "estates": [
                   {
                       "estateid": 11,
                       "name": "Lingzhi New Village",
                       "hot": 20
                   }
               ],
               "name": "Xiao Lili",
               "tel": "13040813886",
               "servstar": 4,
               "realstar": 4,
               "profstar": 4,
               "certificated": false
           }
       ]
   }
   ```

6. Add Real Estate Agent - **POST** `/api/agents/`

   Developer: Luo Hao

   Version: v1

   Description: None.

   Usage: Requires login with administrator privileges. The user identity token is provided in the request header.

   Request Parameters:

   | Parameter    | Type   | Required | Location     | Description              |
   | ------------ | ------ | -------- | ------------ | ------------------------ |
   | name         | String | Yes      | Request Body | Agent name               |
   | tel          | String | Yes      | Request Body | Agent phone number       |
   | servstar     | Integer| No       | Request Body | Default 0                |
   | realstar     | Integer| No       | Request Body | Default 0                |
   | profstar     | Integer| No       | Request Body | Default 0                |
   | certificated | Integer| No       | Request Body | Default 0                |
   | token        | String | Yes      | Request Header | User authentication token |

   Response:

   - Created successfully - Status code **201**:

     ```JSON
     {
         "agentid": 8,
         "estates": [],
         "name": "Sun Xiaomei",
         "tel": "13800991234",
         "servstar": 0,
         "realstar": 0,
         "profstar": 0,
         "certificated": false
     }
     ```

   - Authentication information not provided - Status code **401**:

     ```JSON
     {
         "detail": "Invalid authentication credentials."
     }
     ```

   - Current user does not have permission - Status code **403**:

     ```JSON
     {
         "detail": "You do not have permission to perform this action."
     }
     ```

7. Edit Real Estate Agent Information - **PUT** `/api/agents/{agent_id}/`

    Developer: Luo Hao

    Version: v1

    Description: None.

    Usage: Requires login with administrator privileges. The user identity token is provided in the request header.

    Request Parameters:

    | Parameter    | Type   | Required | Location     | Description              |
    | ------------ | ------ | -------- | ------------ | ------------------------ |
    | name         | String | Yes      | Request Body | Agent name               |
    | tel          | String | Yes      | Request Body | Agent phone number       |
    | servstar     | Integer| No       | Request Body | Default 0                |
    | realstar     | Integer| No       | Request Body | Default 0                |
    | profstar     | Integer| No       | Request Body | Default 0                |
    | certificated | Integer| No       | Request Body | Default 0                |
    | token        | String | Yes      | Request Header | User authentication token |

    Response:

    - Updated successfully - Status code **200**:

     ```JSON
     {
         "agentid": 1,
         "estates": [
             {
                 "estateid": 1,
                 "name": "Today's Garden",
                 "hot": 20
             },
             {
                 "estateid": 2,
                 "name": "Jade Garden",
                 "hot": 30
             },
             {
                 "estateid": 3,
                 "name": "Vanke City Garden",
                 "hot": 22
             }
         ],
         "name": "Yuan Xiaomeng",
         "tel": "158173555285",
         "servstar": 5,
         "realstar": 4,
         "profstar": 3,
         "certificated": true
     }
     ```

    - Authentication information not provided - Status code **403** - Same as Add
    - Current user does not have permission - Status code **403** - Same as Add

8. Delete Real Estate Agent - **DELETE** `/api/agents/{agent_id}/`

    Developer: Luo Hao

    Version: v1

    Description: None.

    Usage: None.

    Request Parameters:

    | Parameter | Type   | Required | Location       | Description              |
    | --------- | ------ | -------- | -------------- | ------------------------ |
    | token     | String | Yes      | Request Header | User authentication token |

    Response:

    - Deleted successfully - Status code **204**
    - Authentication information not provided - Status code **403** - Same as Add
    - Current user does not have permission - Status code **403** - Same as Add

9. Get Estate Listings with Pagination - **GET** `/api/estates/`

    Developer: Luo Hao

    Version: v1

    Description: Agent names are matched using prefix fuzzy matching. Agent service star rating means the agent's service rating must be no lower than the specified rating. Agent certification status has only two options: 0 (not certified) and 1 (certified). The filter conditions represented by the three parameters are combined using AND logic. The result is paginated real estate agent information.

    Usage: None.

    Request Parameters:

    | Parameter | Type   | Required | Location        | Description                        |
    | --------- | ------ | -------- | --------------- | ---------------------------------- |
    | name      | String | No       | Query Parameter | Estate name (fuzzy match)          |
    | dist      | String | No       | Query Parameter | Estate district ID                 |
    | page      | Integer| No       | Query Parameter | Page number, default 1             |
    | size      | Integer| No       | Query Parameter | Page size, default 5, maximum 50   |

    Response:

    ```JSON
    {
        "count": 16,
        "next": "https://120.77.222.217/api/estates/?page=2",
        "previous": null,
        "results": [
            {
                "estateid": 6,
                "district": {
                    "distid": 440303,
                    "name": "Luohu District"
                },
                "agents": [
                    {
                        "agentid": 2,
                        "name": "Yang Wei",
                        "tel": "13352939550",
                        "servstar": 3
                    },
                    {
                        "agentid": 4,
                        "name": "Guo Zhipeng",
                        "tel": "13686810707",
                        "servstar": 4
                    }
                ],
                "name": "Xingfu Li",
                "hot": 300,
                "intro": ""
            }
        ]
    }
    ```

10. Add Estate - **POST** `/api/estates/`

  Developer: Luo Hao

  Version: v1

  Description: None.

  Usage: Requires login with administrator privileges. The user identity token is provided in the request header.

  Request Parameters:

  | Parameter | Type   | Required | Location     | Description                     |
  | --------- | ------ | -------- | ------------ | ------------------------------- |
  | name      | String | Yes      | Request Body | Estate name                     |
  | hot       | Integer| No       | Request Body | Estate popularity, default 0    |
  | intro     | String | No       | Request Body | Estate description, default empty string |
  | distid    | Integer| Yes      | Request Body | Estate district ID              |
  | token     | String | Yes      | Request Header | User authentication token     |

  Response:

  - Created successfully - Status code **201**:
     ```JSON
     {
         "estateid": 17,
         "district": 510107,
         "name": "Century Jinyuan",
         "hot": 100,
         "intro": ""
     }
     ```

  - Authentication information not provided - Status code **403**:
     ```JSON
     {
         "detail": "Please provide valid authentication credentials"
     }
     ```

  - Current user does not have permission - Status code **403**:
     ```JSON
     {
         "detail": "You do not have permission to perform this action."
     }
     ```

11. Edit Estate Information - **PUT** `/api/estates/{estate_id}`

12. Delete Estate - **DELETE** `/api/estates/{estate_id}`

13. Get All House Types - **GET** `/api/housetypes/`

14. Add House Type - **POST** `/api/housetypes/`

15. Edit House Type Information - **PUT** `/api/housetypes/{house_type_id}`

16. Delete House Type - **DELETE** `/api/housetypes/{house_type_id}`

17. Get House Listings with Pagination - **GET** `/api/houseinfos/`

     Developer: Luo Hao

     Version: v1

     Description: None.

     Usage: None.

     Request Parameters:

     | Parameter | Type   | Required | Location        | Description                        |
     | --------- | ------ | -------- | --------------- | ---------------------------------- |
     | title     | String | No       | Query Parameter | House listing title keyword         |
     | dist      | Integer| No       | Query Parameter | Estate district ID                  |
     | min_price | Integer| No       | Query Parameter | Price range lower bound             |
     | max_price | Integer| No       | Query Parameter | Price range upper bound             |
     | type      | Integer| No       | Query Parameter | House type ID                       |
     | page      | Integer| No       | Query Parameter | Page number, default 1              |
     | size      | Integer| No       | Query Parameter | Page size, default 5, maximum 50    |

     Response:
     ```JSON
     {
         "count": 7,
         "next": "http://localhost:8000/api/houseinfos/?dist=440303&page=2",
         "previous": null,
         "results": [

         ]
     }
     ```

18. View House Listing Details - **GET** `/api/houseinfos/{house_id}`

19. Add House Listing - **POST** `/api/houseinfos/`

20. Edit House Listing Information - **PUT** `/api/houseinfos/{house_id}`

21. Delete House Listing - **DELETE** `/api/houseinfos/{house_id}`

22. Randomly Get a Specified Number of House Tags - **GET** `/api/tags/`

23. View House Tags with Pagination - **GET** `/api/tags/`

24. Add House Tag - **POST** `/api/tags/`

25. Delete House Tag - **DELETE**  `/api/tags/{house_id}`

26. View Photos of a House Listing - **GET** `/api/houseinfos/{house_id}/photos/`

27. Add Photo to a House Listing - **POST** `/api/houseinfos/{house_id}/photos/`

28. Delete House Listing Photo - **DELETE** `/api/houseinfos/{house_id}/photos/{photo_id}`
