index=* (EventCode=4624 OR EventCode=4625) | stats count as Attemps, count(eval(EventCode=4625)) as Failed, count(eval(EventCode=4624)) as Success by Account_Name | where NOT like (Account_Name,"%$") AND Failed>1 AND Succes>0|sort by Failed desc 

