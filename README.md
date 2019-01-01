# Manage users and groups in SignalR
SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.
# Users in SignalR
SignalR allows you to send messages to all connections associated with a specific user. By default, SignalR uses the ClaimTypes.NameIdentifier from the ClaimsPrincipal associated with the connection as the user identifier. A single user can have multiple connections to a SignalR app. For example, a user could be connected on their desktop as well as their phone. Each device has a separate SignalR connection, but they're all associated with the same user. If a message is sent to the user, all of the connections associated with that user receive the message. The user identifier for a connection can be accessed by the Context.UserIdentifier property in your hub.

Send a message to a specific user by passing the user identifier to the User function in your hub method as shown in the following example:

```python
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

The user identifier can be customized by creating an IUserIdProvider, and registering it in ConfigureServices.

```python
public class CustomUserIdProvider : IUserIdProvider
{
    public virtual string GetUserId(HubConnectionContext connection)
    {
        return connection.User?.FindFirst(ClaimTypes.Email)?.Value;
    }
}
```
```python
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR();

    services.AddSingleton<IUserIdProvider, CustomUserIdProvider>();
}
```
