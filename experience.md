**Transforming a column**

The `Table.TransformColumns` function takes a list of lists as an argument, each inner list contains a **column name** and the **function** to be applied.

Option 1:  

    #"Transformed null to Now" = Table.TransformColumns(#"Previous Step", {{"stu_timestamp_to", each if _ is null then DateTime.LocalNow() else _ }})

Option 2 (using UDF):


    let
      // This UDF is written as a step
      ClearTableUDF = (tableToClear as table) as table =>
      let
        MinValue = List.Min(tableToClear[siv_islaikymo_data]),
        OnlyFirstLeft = Table.SelectRows(tableToClear, each [siv_islaikymo_data] = MinValue)
      in
      OnlyFirstLeft
    
    #"Transformation using UDF" = Table.TransformColumns(#"Previous Step", {"AllRows", each ClearTableUDF(_)})

Option 3:

    #"Lowercased Text" = Table.TransformColumns(#"Previous Step",{{"col1", Text.Lower, type text}, {"col2", Text.Lower, type text}})

