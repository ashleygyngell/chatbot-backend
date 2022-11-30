
## ChatApp - Solo/Paired Project

Example             |  Homepage
:-------------------------:|:-------------------------:
![mobile-screenshot](https://res.cloudinary.com/dj7e2jadx/image/upload/v1663942253/Phone_Example1_Chat.png)  |  ![ipad-screenshot](https://res.cloudinary.com/dj7e2jadx/image/upload/v1663942323/Tablet_Example_Chat.png)


## Deployment 

This app has been deployed via Heroku and Netlify and is available [here](https://chatwithchatapp.netlify.app/).

The free servers on Heroku sleep when they are not in use, so please allow a few seconds for them to wake up!

Feel free to register as a user or use: **email:** `demo@demo.com` **password:** `Demo123!`

This Repo is for the backend only. The code for the frontend is available [here](https://github.com/ashleygyngell/chatapp-frontend).

# Languages / Frameworks / Databases used

- Django
- Python
- PostgreSQL
- React
- Javascript
- Bulma
- HTML5

# Concept

The project was concieved in collaboration with [Edward Foulds](https://github.com/FouldsEJ), who helped build this backend. 

Our idea was as follows: 

- Build a full stack messaging application.
- Using an Express API to serve up data from a Django database.
- Consume the API with a separate front-end built with React.
- Take inspiration from iMessage 

## Phase One (Day 1) 

**ERD & Whiteboarding**

We used an entiry relationship diagram to visualise our backend structure. We decided to use Django and a PostgreSQL database as they best suited our relational and structured data. 

The following relationships were established: 

- Users can have many chatrooms & chatrooms can have many users.
- Chatrooms can also have many messages.
- Individual messages only have one room and one user. 

## Phase Two (Days 2-3) 

**Python, Django and a SQL database** 

We added tickets to a trello board (e.g create a model for the chat or write the serializer for a chatroom) that represented the tasks we needed to complete. While we tackled each ticket individually, we worked virtually via zoom, regularly checking in to ask each other for advice and made sure to  commit every completede task to the shared git repository. 

Below is a small example of the ChatView that I built during this timeframe. 

```
class ChatView(RetrieveUpdateDestroyAPIView):
   permission_classes = [IsAuthenticated, ]
   def get(self, _request, pk):

    obj = list(Chat.objects.filter(ChatRoomID=pk))
    serializer_class = ChatSerializer(obj, many=True)

    return JsonResponse( {'data': serializer_class.data})
```

Lets say the request from the client was to 'retrieve' data, the app first checks if the user is authenticated by accessing the bearer token from the browser (set once the user logs in - shown below). The chat objects (many) are then returned as serialized data to the client. 

A snippet of the authentication process is as follows: 

```
class JWTAuthentication(BasicAuthentication):

    def authenticate(self, request):
        header = request.headers.get('Authorization')
        if not header:
            return None

        if not header.startswith('Bearer'):
            raise PermissionDenied({'message': 'Invalid authorization header'})

        token = header.replace('Bearer ', '')
```

## Phase Three (Days 4-7) 

**Building the Front End** 

After successfully testing the API endpoints in Postman and the work with Ed was complete, I began building the Front End. 

One component that was a really rewarding challenge was displaying the messages within each chatroom. To execute this, I used a ternary statement to check for the chat data, which if present, returned the conversation. This was then mapped over and displayed with the sent messages being displayed on the left and the recieved messages to the right. 

To do this, for each message (shown below as 'data' in line 6) , I checked if the 'sender id' strictly equalled the 'userId' (stored as a browser token, in a similar fashion to the authentication process). 

To demonstrate - when checking for a sent message, the code block reads as follows: 

``` 
!chatData ? (
 <p>Loading chat...</p>
   ) : (
       <section className="columns">
         <div className="column">
           {chatData.data.map((data) => {
             if (data.sender_id.id === userId) {
               // console.log(data.creation_time.split('T')[0]);
               return (
                   <div>
                     <div className="sent-message" key={data.chat_id}>
                       <p>{data.message}</p>
                       <span className="message-data">
                         {data.creation_time}
                       </span>
                     </div>
                   </div>
               );
             } else {
```


## Wins

- Creating a full stack messaging application in 7 days. 
- Working in collaboration with Ed for the backend build further developed my understanding of a PostgreSQL database.
- Django has very useful 'out of the box' features.
- Build a simple, yet logical design for the frontend.

## Challenges / Bugs

- Sometimes when a user sends a message, the useEffect hook doesnt always refresh the page to display it immediately. 

## Stretch Goals

- Fix the above issue.
- Send notifications to a user when they have recived a new message, a friend has created a new chatroom, or has added the user to a chatroom. 

## Takeaways

- This is the first project where I have worked both as a collaborator and solo, which made me appreciate the creativity, knowledge and growth that comes from building a project as part of a team. 
