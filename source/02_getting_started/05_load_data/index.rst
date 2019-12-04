.. _load_data:

Step 4: Load Data
=====================


.. warning:: WIP

Step number 4 brings us to the core functionality of openEHR: creating and storing clinical data! For this purpose,
we will re-use the Template that we created in step 1. As data instances in openEHR are stored as instances of its
Reference Model, it's rather difficult to read for humans. However, this level of abstraction is needed inside the
backend to achieve high scalability. 

For application developers, more accessible formats are needed. The EHRbase Client Library allows to use a Template 
(as OPT) as input and automatically create java classes. These can then be used to create the data. We explain this
processes step by step. We assume that you have successfully built the Client Library.

Firstly, you need to create the Java classes from the OPT. This could look like as follows:

.. code-block:: javascript

			java -jar client-library-0.2.0.jar -opt "C:\Users\MyUser\Desktop\HiGHmed_Cardio_Monitoring_v1.opt" -out "C:\openEHR SDK\ehrbase_client_library\src\test\java\org\ehrbase\client\classgenerator" -package ""

You should find a file named *HiGHmed_Cardio_Monitoring_v1.java* inside your project structure that should look like this:

.. code-block:: Java

                ...
                @Entity
                @Archetype("openEHR-EHR-COMPOSITION.self_monitoring.v0")
                @Template("HiGHmed_Cardio_Monitoring.v1")
                public class HighmedCardioMonitoringV1 {
                 @Path("/context/end_time|value")
                 private TemporalAccessor endTimeValue;

                 @Path("/language")
                 private CodePhrase language;

                 @Path("/context/health_care_facility")
                 private PartyIdentified healthCareFacility;

                 @Path("/composer|external_ref")
                 private PartyRef composerExternalref;

                ...

Next, we can create a new test function like this:

.. code-block:: Java

        public static HighmedCardioMonitoringV1 buildCardioExample(){

         //Create the composition instance and add metadata
         HighmedCardioMonitoringV1 cardioMonitoring = new HighmedCardioMonitoringV1();
         cardioMonitoring.setLanguage(new CodePhrase(new TerminologyId("ISO_639-1"), "de"));
         cardioMonitoring.setTerritory(new CodePhrase(new TerminologyId("ISO_3166-1"), "DE"));
         cardioMonitoring.setSettingDefiningcode(new CodePhrase(new TerminologyId("openehr"), "229"));
        
         //Create a blood pressure object 
         HighmedCardioMonitoringV1.Blutdruck bloodpressure = new HighmedCardioMonitoringV1.Blutdruck();
        
         //Add data for systolic and diastolic blood pressure
         bloodpressure.setSystolischMagnitude(160d);
         bloodpressure.setSystolischUnits("mm[HG]");
        
         bloodpressure.setDiastolischMagnitude(120d);
         bloodpressure.setDiastolischUnits("mm[HG]");

         //Add data for a medical device
         HighmedCardioMonitoringV1.Blutdruck.MedizingerT geraet = new HighmedCardioMonitoringV1.Blutdruck.MedizingerT();
         geraet.setBeschreibungValue("OMRON Sensor");

         DvIdentifier identifier = new DvIdentifier();
         identifier.setId("4567879799");
         geraet.setEindeutigeIdentifikationsnummerId(identifier);

         List<HighmedCardioMonitoringV1.Blutdruck> bpList = new ArrayList<>();
         bpList.add(bloodpressure);
        
         cardioMonitoring.setBlutdruck(bpList);
        
         return cardioMonitoring;
     }
	 
Finally, the composition can be sent to the openEHR server:

.. code-block:: Java

        CompositionEndpoint compositionEndpoint = openEhrClient.compositionEndpoint(ehr);
        UUID compositionId = compositionEndpoint.saveCompositionEntity(highmedCardioMonitoringV1);

Congratulations, you stored your first clinical data inside EHRbase! Next, we will take a look how 
we can retrieve the data using the Archetype Query Language. 