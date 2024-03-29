# Abstraction

#### Duplicate Abstraction
  - having same name 
  - most of the code is shared by the abstractions

  

// problem: the ReadBackupHeader is duplicated in both get and perform method with identical responsibility.

```cs
class GetDataRestoreExecutor
{
    //
    BackupHeader ReadBackupHeader(string headerFileName)
    {
        // open the headerFileName and read the data in stream into backupheader
        return new();
    }
}
class PerformDataRestoreExecutor
{
    BackupHeader ReadBackupHeader(string headerFileName)
    {
        // open the headerFileName and read the data in stream into backupheader
        return new();
    }
}

class BackupHeader{}

```
// Solution: use an abstraction for ReadBackupHeader (interface or abstract class)

#!markdown

##### imperative abstraction - operation is turned into class

#!csharp

// Imperative abstraction
public class HILConfigurationConverter
{
    public HILConfiguration ConvertTo(HILIndexConfigSet configset)
    {
           // conversion logic
           return new();
    }
}
public class HILConfiguration
{

}
public class HILIndexConfigSet
{
    
}

// Solution: use the mapper used in project instead of a saperate class

#!markdown

##### Multifacted abstraction

handler has more than one respnsibility - handles multiple commands at a time

#!csharp

public class TDefHandler // is it a multifacted abstraction or unexploited encapsulation
{
    public async Task<Response<T>> ExecuteCommand<TRequest, T>(TRequest request)
    {
        Response<BusResponseEx> response = new Response<BusResponseEx>();
        response.Message = new BusResponseEx();
        TdefRequest tdefRequest = JsonConvert.DeserializeObject<TdefRequest>(request.ToString());
        try
        {
            switch (tdefRequest.ActionType)
            {
                case ActionType.GET_TDEF_SINGLE:
                    {
                        
                    }
                case ActionType.FILTER_TDEF:
                    {
                        
                    }
                case ActionType.GET_REPEAT_CONDITION:
                    {
                       
                    }
                case ActionType.UPSERTREFLEXGROUPS:
                    {
                        
                    }
                case ActionType.UPSERTREPEAT:
                    {
                        
                    }
                case ActionType.UPSERTTDEF:
                    {
                        
                    }
                case ActionType.GETG0TDEF:
                    {
                        

                    }

                case ActionType.VALIDATEDELETE:
                    {
                        
                    }
                case ActionType.DELETE:
                    {
                     
                    }
                case ActionType.RESTORETODEFAULT:
                    {
                       
                    }
                case ActionType.GETBACKUPSTATE:
                    {
                      
                    }
                case ActionType.IMPORTSINGLE:
                    {
                        
                    }
                case ActionType.IMPORTMULTI:
                    {
                       
                    }
                case ActionType.SAVEMULTIIMPORT:
                    {
                        
                    }
                case ActionType.CANCEL_TDEF_IMPORT:
                    {
                        
                    }
                case ActionType.VALIDATE_TDEF_UNIT_CONVERSION:
                    {
                       
                    }
                case ActionType.PERFORM_TDEF_UNIT_CONVERSION:
                    {
                       
                    }
                case ActionType.GET_TDEF_UNIT_CONVERSION:
                    {
                       
                    }
                case ActionType.UNIT_CHANGE_TDEF:
                    {
                        
                    }
                case ActionType.GET_MASTERCURVE_USAGE:
                    {
                        
                    }
                case ActionType.IMPORT_BLOB:
                    {
                       
                    }

                case ActionType.VALIDATE_COPY_TEST:
                    {
                       
                    }
            }

        }
        catch (Exception e)
        {
            
        }
        
    } 

    // Solution: use command executor for each of these methods and just invoke the corrosonding executors
}

#!markdown

##### Unuitlized abstraction 

 - DisableUnsupportedA1CAssay method in ModuleConfigurationManagerCC - changing the assay state but not utilized anymore

#!markdown

# Encapsulation

#!markdown

#####  Deficient Encapsulation
`access` more than required

#!csharp

public class UserPromptActionExecutor
{
    public static string SuccessMsg = "User prompt action for Consumable {0} with user action {1} conveyed to Module Successfully";
    public static string FailedMsg = "User prompt user action to Module unsuccessfull.";

    // these two static fields are used internally and not required to be exposed with public access modifier
}

public class ConsumableDoorOpenUserActionExecutor
{
    public static string SuccessMsg = "Consumable {0} Door Close UserAction {1} conveyed to Module Successfully";
    public static string FailedMsg = "Consumable Door Close UserAction to Module unsuccessfull.";
}

#!markdown

##### Leaky Encapsulation
- abstraction exposes or leaks implementation, through public interfaces.

#!csharp

public class OpenChannelManager
{
    // The method is marked as internal 
    // By default, classes are `internal` in scope - meaning that they can only be interacted with by other classes in the same library or program
    // but this method is only used with in the same class.
    internal List<OpenChannels> GetOpenChannels()
        {
            return new ();
            //return _openChannelProxy.GetMaxOpenChannels();
        }
}
public class OpenChannels
{

}

// found such examples of internal in CalibrationDataHandler.cs class- these methods are marked to internal to be exposed for unit test classes

#!markdown

# Hirerarchy

##### Rebilous Hierarchy

#!csharp

public class TDefHandler: AbstractHandler
{
    public override void OnRefresh()
        {
            throw new NotImplementedException();
        }
        public override void OnStop()
        {
            throw new NotImplementedException();
        }
}

public abstract class AbstractHandler
{
  public abstract void OnRefresh();
  public abstract void OnStop();   
}

#!markdown

### SRP - Single Responsibility Principle

#!csharp

public class OpenChannelManaager
{
    public BusResponseEx ValidateAndSaveOpenChannelAssay(OpenChannelTdef openChannelTDef, OpenChannelTdef existingOpenChannelTDefInCache,
        Dictionary<int, string> cachedDonors, List<OpenChannelTdef> openChannelTdefs,
        bool canEnableNegativeValueEntry, IEnumerable<UDD.Common.Models.ModuleStateInfo> moduleStates, bool isCalibrationInProgress)
        {
            // multiple responsibilites and missigng abstraction
            // Multiple if and else to validate input data and save it to module
        }

        // Solution :Spilit the method and saggeragate responsiblity
}
