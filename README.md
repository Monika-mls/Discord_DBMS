# Discord_DBMS

-- Users Table
CREATE TABLE user (
  User_id int NOT NULL,
  Username varchar(45) NOT NULL,
  Email varchar(45) DEFAULT NULL,
  Join_date date NOT NULL,
  Role_id int NOT NULL,
  Server_id int NOT NULL,
  PRIMARY KEY (User_id),
  UNIQUE KEY Username_UNIQUE (Username),
  KEY role_id_fk_user_idx (Role_id),
  KEY server_id_fk_user_id (Server_id),
  CONSTRAINT role_id_fk_user FOREIGN KEY (Role_id) REFERENCES roles (Role_id),
  CONSTRAINT server_id_fk_user FOREIGN KEY (Server_id) REFERENCES servers (Server_id)
);

-- Servers Table
CREATE TABLE servers (
  Server_id int NOT NULL,
  Server_name varchar(45) NOT NULL,
  Owner_id int NOT NULL,
  PRIMARY KEY (Server_id),
  UNIQUE KEY Server_name_UNIQUE (Server_name),
  KEY owner_id_fk_ser_idx (Owner_id),
  CONSTRAINT owner_id_fk_ser FOREIGN KEY (Owner_id) REFERENCES users (User_id)
);

-- Roles Table
CREATE TABLE roles (
  Role_id int NOT NULL,
  Role_name varchar(45) NOT NULL,
  Server_id int DEFAULT NULL,
  PRIMARY KEY (Role_id),
  KEY server_id_fk_role_idx (Server_id),
  CONSTRAINT server_id_fk_role FOREIGN KEY (Server_id) REFERENCES servers (Server_id)
);

-- Channels Table
CREATE TABLE channels (
  Channel_id int NOT NULL,
  Channel_name varchar(45) NOT NULL,
  Server_id int DEFAULT NULL,
  Channel_Type varchar(45) NOT NULL,
  PRIMARY KEY (Channel_id),
  UNIQUE KEY Channel_name_UNIQUE (Channel_name),
  KEY server_id_idx (Server_id),
  CONSTRAINT server_id_fk FOREIGN KEY (Server_id) REFERENCES servers (Server_id)
);

-- Messages Table
CREATE TABLE messages (
  Message_id int NOT NULL,
  User_id int DEFAULT NULL,
  Channel_id int DEFAULT NULL,
  Content varchar(45) NOT NULL,
  TimeStamp datetime NOT NULL,
  PRIMARY KEY (Message_id),
  KEY user_id_fk_idx (User_id),
  KEY channel_id_fk_idx (Channel_id),
  CONSTRAINT channel_id_fk FOREIGN KEY (Channel_id) REFERENCES channels (Channel_id),
  CONSTRAINT user_id_fk FOREIGN KEY (User_id) REFERENCES users (User_id)
) 

-- Inserting values into Users table
INSERT INTO users VALUES ('1', 'Alice123', 'alice@mail.com', '2024-01-05', '201', '101');
INSERT INTO users VALUES ('2', 'Bobinator', 'bob@mail.com', '2024-02-10', '203', '102');
INSERT INTO users VALUES ('3', 'Charlie_C', 'charlie@mail.com', '2024-03-15', '202', '101');
INSERT INTO users VALUES ('4', 'DaisyD', 'daisy@mail.com', '2024-04-10', '204', '101');
INSERT INTO users VALUES ('5', 'EveEcho', 'eve@mail.com', '2024-05-05', '205', '103');
INSERT INTO users VALUES ('6', 'FrankyF', 'frank@mail.com', '2024-06-20', '203', '102');
INSERT INTO users VALUES ('7', 'GamerGal', 'gamer@mail.com', '2024-07-12', '206', '102');
INSERT INTO users VALUES ('8', 'HackerHank', 'hank@mail.com', '2024-08-01', '201', '101');
INSERT INTO users VALUES ('9', 'IndieIvan', 'ivan@mail.com', '2024-08-15', '207', '104');
INSERT INTO users VALUES ('10', 'JackieJ', 'jackie@mail.com', '2024-09-10', '202', '101');

-- Inserting values into Servers table
INSERT INTO servers VALUES ('101', 'DevHub', '1');
INSERT INTO servers VALUES ('102', 'GameWorld', '2');
INSERT INTO servers VALUES ('103', 'ArtStation', '5');
INSERT INTO servers VALUES ('104', 'IndieDevelopers', '9');
INSERT INTO servers VALUES ('105', 'MusicLounge', '7');
INSERT INTO servers VALUES ('106', 'CodeCrafters', '8');
INSERT INTO servers VALUES ('107', 'MovieClub', '3');
INSERT INTO servers VALUES ('108', 'WritersCircle', '4');
INSERT INTO servers VALUES ('109', 'ScienceLab', '6');
INSERT INTO servers VALUES ('110', 'FitnessFocus', '10');

-- Inserting values into Channels table
INSERT INTO channels VALUES ('301', 'General', '101', 'text');
INSERT INTO channels VALUES ('302', 'Help Desk', '101', 'text');
INSERT INTO channels VALUES ('303', 'Game Lobby', '102', 'voice');
INSERT INTO channels VALUES ('304', 'Art Showcase', '103', 'text');
INSERT INTO channels VALUES ('305', 'Coding Help', '104', 'text');
INSERT INTO channels VALUES ('306', 'Music Room', '105', 'voice');
INSERT INTO channels VALUES ('307', 'Movie Discussion', '107', 'text');
INSERT INTO channels VALUES ('308', 'Writing Prompts', '108', 'text');
INSERT INTO channels VALUES ('309', 'Science News', '109', 'text');
INSERT INTO channels VALUES ('310', 'Workout Room', '110', 'voice');

-- Inserting values into Messages table
INSERT INTO messages VALUES ('401', '1', '301', '\"Hello everyone!\"', '2024-03-20 10:15:00');
INSERT INTO messages VALUES ('402', '2', '303', '\"Ready for the game?\"', '2024-03-20 10:17:00');
INSERT INTO messages VALUES ('403', '3', '302', '\"Can I get some help?\"', '2024-03-21 09:45:00');
INSERT INTO messages VALUES ('405', '5', '304', '\"Check out my latest art!\"', '2024-03-21 11:30:00');
INSERT INTO messages VALUES ('406', '6', '309', ':Exciting news in science\"', '2024-03-21 12:00:00');
INSERT INTO messages VALUES ('407', '7', '306', '\"Let\'s jam together!\"', '2024-03-21 12:15:00');
INSERT INTO messages VALUES ('408', '8', '305', '\"Need help with this code\"', '2024-03-21 14:00:00');
INSERT INTO messages VALUES ('409', '9', '307', '\"Anyone seen this movie?\"', '2024-03-21 15:00:00');
INSERT INTO messages VALUES ('410', '10', '310', '\"Workout tips?\"', '2024:03:21 16:00:00');

-- Inserting values into Roles table
INSERT INTO roles VALUES ('201', 'Admin', '101');
INSERT INTO roles VALUES ('202', 'Moderator', '101');
INSERT INTO roles VALUES ('203', 'Gamer', '102');
INSERT INTO roles VALUES ('204', 'Helper', '101');
INSERT INTO roles VALUES ('205', 'Artist', '103');
INSERT INTO roles VALUES ('206', 'Player', '102');
INSERT INTO roles VALUES ('207', 'Developer', '104');
INSERT INTO roles VALUES ('208', 'Musician', '105');
INSERT INTO roles VALUES ('209', 'Writer', '108');
INSERT INTO roles VALUES ('210', 'Scientist', '109');

-- Queries

Query 1: List All Users Along with Their Roles and Server Names.
SELECT Users.username, Roles.role_name, Servers.server_name
FROM Users
JOIN Roles ON Users.role_id = Roles.role_id
JOIN Servers ON Users.server_id = Servers.server_id;

Query 2: List All Channels in a Specific Server.
SELECT Channels.channel_name, Channels.channel_type
FROM Channels
JOIN Servers ON Channels.server_id = Servers.server_id
WHERE Servers.server_name = 'DevHub';

Query 3: Retrieve All Messages Sent in a Specific Channel.
SELECT Messages.content, Users.username, Messages.timestamp
FROM Messages
JOIN Users ON Messages.user_id = Users.user_id
JOIN Channels ON Messages.channel_id = Channels.channel_id
WHERE Channels.channel_name = 'General';

Query 4: List All Roles in a Server Along with the Count of Users Having Each Role.
SELECT Roles.role_name, COUNT(Users.user_id) AS user_count
FROM Roles
JOIN Users ON Roles.role_id = Users.role_id
JOIN Servers ON Roles.server_id = Servers.server_id
WHERE Servers.server_name = 'DevHub'
GROUP BY Roles.role_name;

Query 5: Retrieving Details of the Most Recent Message Sent in Each Channel.
SELECT Channels.channel_name, Messages.content, Users.username, Messages.timestamp
FROM Messages
JOIN Channels ON Messages.channel_id = Channels.channel_id
JOIN Users ON Messages.user_id = Users.user_id
WHERE (Messages.channel_id, Messages.timestamp) IN (
    SELECT channel_id, MAX(timestamp)
    FROM Messages
    GROUP BY channel_id
)
ORDER BY Channels.channel_name;

Query 6: Find the User with the Most Messages Across All Servers.
SELECT Users.username, COUNT(Messages.message_id) AS total_messages
FROM Messages
JOIN Users ON Messages.user_id = Users.user_id
GROUP BY Users.username
ORDER BY total_messages DESC
LIMIT 1;

Query 7: List All Roles and Their User Counts in Each Server.
SELECT Roles.role_name, Servers.server_name, COUNT(Users.user_id) AS user_count
FROM Roles
JOIN Users ON Roles.role_id = Users.role_id
JOIN Servers ON Users.server_id = Servers.server_id
GROUP BY Roles.role_name, Servers.server_name
ORDER BY Servers.server_name, Roles.role_name;

Query 8: Find the Most Active Channel (by Message Count) in Each Server.
SELECT Servers.server_name, Channels.channel_name, COUNT(Messages.message_id) AS message_count
FROM Messages
JOIN Channels ON Messages.channel_id = Channels.channel_id
JOIN Servers ON Channels.server_id = Servers.server_id
GROUP BY Servers.server_id, Channels.channel_id
HAVING COUNT(Messages.message_id) = (
    SELECT MAX(channel_count) 
    FROM (
        SELECT COUNT(message_id) AS channel_count
        FROM Messages AS M
        JOIN Channels AS C ON M.channel_id = C.channel_id
        WHERE C.server_id = Servers.server_id
        GROUP BY M.channel_id
    ) AS counts
);

Query 9: Find Users Who Are in Multiple Servers and List Their Roles in Each Server.
SELECT Users.username, Servers.server_name, Roles.role_name
FROM Users
JOIN Roles ON Users.role_id = Roles.role_id
JOIN Servers ON Users.server_id = Servers.server_id
WHERE Users.user_id IN (
    SELECT user_id
    FROM Users
    GROUP BY user_id
    HAVING COUNT(DISTINCT server_id) > 1
)
ORDER BY Users.username, Servers.server_name;

Query 10: Retrieve All Messages Sent by Users with the Role "Admin" in a Specific Server.
SELECT Messages.content, Users.username, Channels.channel_name
FROM Messages
JOIN Users ON Messages.user_id = Users.user_id
JOIN Roles ON Users.role_id = Roles.role_id
JOIN Channels ON Messages.channel_id = Channels.channel_id
JOIN Servers ON Channels.server_id = Servers.server_id
WHERE Roles.role_name = 'Admin' AND Servers.server_name = 'DevHub';

Query 11: Creating a View on  Active Members per Server .This view will show the number of users per server who joined in the last month.
CREATE VIEW ActiveMembers AS
SELECT
    S.server_id,
    S.server_name,
    COUNT(U.user_id) AS active_member_count
FROM
    Servers S
JOIN
    Users U ON S.server_id = U.server_id
WHERE
    U.join_date >= DATEADD(MONTH, -1, GETDATE())
GROUP BY
    S.server_id, S.server_name;

Query 12: Using a View and Aggregate Functions on Top 5 Active Channels. This view calculates the top 5 channels with the highest message count.
CREATE VIEW TopActiveChannels AS
SELECT
    C.channel_id, C.channel_name, C.server_id,
    COUNT(M.message_id) AS message_count
FROM Channels C
JOIN
    Messages M ON C.channel_id = M.channel_id

GROUP BY
    C.channel_id, C.channel_name, C.server_id
ORDER BY message_count DESC LIMIT 5;

Query 13: Trigger to Restrict Deletion of Server Owners .This trigger prevents the deletion of a user if they are currently the owner of any server.
CREATE TRIGGER PreventOwnerDeletion
ON Users
INSTEAD OF DELETE
AS
BEGIN
    DECLARE @user_id INT;
    SELECT @user_id = user_id FROM deleted;
    IF EXISTS (SELECT 1 FROM Servers WHERE owner_id = @user_id)
    BEGIN
        PRINT 'Cannot delete user because they are a server owner.';
    END
    ELSE
    BEGIN
        DELETE FROM Users WHERE user_id = @user_id;
    END
END;

Query 14: Cursor for Sending Notifications to Inactive Users. This stored procedure uses a cursor to iterate over users who have been inactive (no messages sent) for over a year. It would send a notification to each inactive user.
CREATE PROCEDURE NotifyInactiveUsers AS
BEGIN
    DECLARE @user_id INT;
    DECLARE @username VARCHAR(50);
    DECLARE @email VARCHAR(100);
    DECLARE inactive_cursor CURSOR FOR
    SELECT U.user_id, U.username, U.email
    FROM Users U
    LEFT JOIN Messages M ON U.user_id = M.user_id
    WHERE M.timestamp IS NULL OR DATEDIFF(YEAR, M.timestamp, GETDATE()) >= 1;
    OPEN inactive_cursor;
    FETCH NEXT FROM inactive_cursor INTO @user_id, @username, @email;
    WHILE @@FETCH_STATUS = 0
    BEGIN
        PRINT 'Sending notification to ' + @username + ' (' + @email + ')';
        FETCH NEXT FROM inactive_cursor INTO @user_id, @username, @email;
    END;
    CLOSE inactive_cursor;
    DEALLOCATE inactive_cursor;
END;

