EventBus workflow diagram

```
                                                                                          
                                                        |--->                             
                                                        |                                 
                                       +-------+        |                                 
                                       +-------+        |--->                             
              -                 (2)    |       | <---- ^|                                 
                            save(event)| (ETS) | ----> ||                                 
                                      >| Event |     | ||--->                             
                                     / | Store |     | ||                                 
                                   -/  |       |     | ||                                 
                                  /    +-------+     | ||--->                             
                                 /     +-------+     | ||                                 
                                /          ^         | ||                                 
     (1)      +------------+  -/           |         | ||--->(4)                          
 notify(event)|            | /             |         | ||                                 
 ------------>|  EventBus  |/         (8) delete     | ||                                 
              |            |-\         +-------+     | ||                                 
              +------------+  -\       |       |     | ||--->                             
                                --\    | (ETS) |     | ||                                 
                                   -\    Event |     | ||                                 
                                     ->|Watcher|     | ||--->                             
                      save(subscribers)|       |     | ||                                 
                               (3)     |       |     | ||                                 
                                       |       |     | ||--->                             
                                       +-------+     | ||                                 
                                           ^         | ||      +----------------+         
                                           |         | ||--->  |SampleSubscriber|         
                                           |         | |       +----------------+         
                                           |         | |   (5)  |  ^  |                   
                                           |         v <------- v  |  |                   
                                           |         ------------->   |                   
                                           |                          |                   
                                           |                          |                   
                                           | <----------------------- v                   
                                              (6) mark_as_completed                       
                                                                                          
                                                                                          
 (1) * EventBus.notify(%EventBus.Model.Event{})                                           
 (2) EventBus.Manager.Store.save(event) # ETS insert eb_es_<<topic>>                      
 (3) EventBus.Manager.Observation.save(even_id, subscribers) # ETS instert eb_ew_<<topic>>
 (4) EvenBus calls process/1 of all subscribers                                           
 (5) * Subscriber calls EventBus.fetch_event({topic, event_id})                           
 (6) * Subscriber calls EventBus.mark_as_completed({topic, event_id})                     
 (7) EventBus.Manager.Observation checks if all subscribers processed the event           
 (8) EventBus.Manager.Store.delete({topic, event_id})                                     
                                                                                          
 (*) Function calls need to be implemented in your app, the rest is done by EventBus      

ASCII workflow created with https://textik.com/
```

