# ApiLog

Remember to change IP address in Program.fs

To use with F#, here is code example:
``` fsharp
module ApiLog =
  let httpClient = new HttpClient()
  
  let log (message: string) =
    async {
      do! Async.SwitchToThreadPool()
  
      let url = "http://192.168.1.2:5001/log"
  
      do! httpClient.PostAsync(url, new StringContent(message)) |> Async.AwaitTask |> Async.Ignore
    } |> Async.Start
  
  let rec logException (e: exn) =
    log ^ e.GetType().Name
    log e.Message
    log e.StackTrace
    e.InnerException
    |> Option.ofObj
    |> Option.iter logException
```

Remember to replace IP address as well
