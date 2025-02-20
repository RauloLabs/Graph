# Define the user and time range
$user = "user@contoso.com"
$startDate = (Get-Date).AddDays(-7).ToString("yyyy-MM-ddTHH:mm:ssZ")
$endDate = (Get-Date).ToString("yyyy-MM-ddTHH:mm:ssZ")

# Define the Graph API endpoint
$endpoint = "https://graph.microsoft.com/v1.0/users/$user/events?\$filter=start/dateTime ge '$startDate' and end/dateTime le '$endDate'"
#Method>>GET /users/{id | userPrincipalName}/calendarView?startDateTime={start_datetime}&endDateTime={end_datetime}
#Reff>>https://learn.microsoft.com/en-us/graph/api/user-list-calendarview?view=graph-rest-1.0&tabs=http


# Define your Azure AD app credentials
$tenantId = "YOUR_TENANT_ID"
$clientId = "YOUR_CLIENT_ID"
$clientSecret = "YOUR_CLIENT_SECRET"

# Get the access token
$body = @{
    grant_type    = "client_credentials"
    scope         = "https://graph.microsoft.com/.default"
    client_id     = $clientId
    client_secret = $clientSecret
}

$tokenResponse = Invoke-RestMethod -Method Post -Uri "https://login.microsoftonline.com/$tenantId/oauth2/v2.0/token" -ContentType "application/x-www-form-urlencoded" -Body $body
$accessToken = $tokenResponse.access_token

# Make the API request
$response = Invoke-RestMethod -Uri $endpoint -Headers @{Authorization = "Bearer $accessToken"}

# Display the results
$response.value | ForEach-Object {
    [PSCustomObject]@{
        Subject = $_.subject
        Start = $_.start.dateTime
        End = $_.end.dateTime
        Location = $_.location.displayName
    }
} | Format-Table -AutoSize
