## IAM(Identity and Access Management)
- Global service
- Service to manage users and groups
- When an account is created, by default a root user is assigned. Best practice is to use it only for setting up the account. This user shouldn't be shared or used in actuality.
- Users are people within your organization, and could be grouped. 
    - Consider Charles, Bob and Alice are developers whereas Mike and Molly are testers. Charles, Bob and Alice can be grouped and put under "Developers" group and Mike, Molly can be grouped and put under "Testers" group implying clear segregation.
- Groups can only contain users, not other groups
- User don't have to belong to any group. This is generally NOT a best practice as permissions are defined at a group level for most of the business use cases.
- User can belong to multiple groups.
    - In the case of Shelly, a brown dusky girl who could develop and very well involve in testing, she could be put under both "Developers" and "Testers" group

#### IAM: Permissions
- Users and groups can be assigned JSON documents called policies.
- Policies help define the permissions of the users.
- Always follow the principle of least privilege. As in, define permissions at a granular level and only grant the required permissions across groups.
    - Below is an example policy that is targeting the resource ***arn:aws:dynamodb:*:*:table/MyTable*** which could be assigned to a group or a user.
    ```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "ListAndDescribe",
                "Effect": "Allow",
                "Action": [
                    "dynamodb:List*",
                    "dynamodb:DescribeReservedCapacity*",
                    "dynamodb:DescribeLimits",
                    "dynamodb:DescribeTimeToLive"
                ],
                "Resource": "*"
            },
            {
                "Sid": "SpecificTable",
                "Effect": "Allow",
                "Action": [
                    "dynamodb:BatchGet*",
                    "dynamodb:DescribeStream",
                    "dynamodb:DescribeTable",
                    "dynamodb:Get*",
                    "dynamodb:Query",
                    "dynamodb:Scan",
                    "dynamodb:BatchWrite*",
                    "dynamodb:CreateTable",
                    "dynamodb:Delete*",
                    "dynamodb:Update*",
                    "dynamodb:PutItem"
                ],
                "Resource": "arn:aws:dynamodb:*:*:table/MyTable"
            }
        ]
    }
    ```
#### Access Type
- Programmatic access
    - Enables an access key ID and secret access key for the AWS API, CLI, SDK, and other development tools.
    - Granted for applications that needs AWS access.
- AWS Management Console access
    - Enables a password that allows users to sign-in to the AWS Management Console.
    - Granted to people to login to the AWS console.

#### IAM: Policies
- Policies could be attached at group level.
    - For example, if a policy is attached to "Developers" group, all the users in the "Developers group" will inherit the policies.
##### Policy Structure
- Consists of:
    - Version - policy language version. Ex: "Version": "2012-10-17"
    ```
    "Version": "2012-10-17"
    ```
    - Id - identifier for the policy(optional).
    - Statements - one or more individual statements(Required).
- Each statement consists of:
    - Sid: identifier for the statement(optional).
    - Effect: whether the statement allows or denies access(Allow, Deny)
    - Principal: user/group/role to which this policy is applied to.
    - Action: list of actions this policy allows or denies.
    - Resource: list of resources to which the actions are applied to.
    - Conditions: Conditions for when the policy should be in effect.(Optional)
    - Example:
        ```
        {
                "Sid": "ListAndDescribe",   // Sid
                "Effect": "Allow",          // Effect - Allow
                "Action": [                 // List of actions
                    "dynamodb:List*",
                    "dynamodb:DescribeReservedCapacity*",
                    "dynamodb:DescribeLimits",
                    "dynamodb:DescribeTimeToLive"
                ],
                "Resource": "*"             // Resources(In this case, all the resources)
            }
        ```
