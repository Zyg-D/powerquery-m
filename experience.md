**Transforming a column**

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
