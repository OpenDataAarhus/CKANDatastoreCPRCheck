
Check date where come from the CKAN datastore API, for the danish personally identifiable number.

Setup
--------------
Replace the exsisterende action.py file or insert the following code.

def validcpr( data_dict ):
    resource_id=data_dict["resource_id"]
    ignore=read_ignore_list()

    if ignore.find(str(resource_id))>-1:
        return False
    records=str(data_dict["records"])
    p = re.compile(r"\d{5,6}[ -]\d{4}")
    m = p.search(records)

    match=False
    if m:
        match=True
        cpr=m.group()
        if len(cpr)==11:
            day=cpr[0:2]
            month=cpr[2:4]
        else:
            day=cpr[0:1]
            month=cpr[1:3]
        if (int(day)>31):
            match=False
        if int(month)>12:
            match=False
    return match

def read_ignore_list():
    ignore="""
	    36b4268b-4c43-48c7-a74f-678cf9f3af8d,
        caf6cb0f-a241-459b-a045-a54fad7ebe3a
    """
    return ignore

def cprError():
    raise p.toolkit.ValidationError({
        'records': ['Fields may contain personally identifiable information.']
    })
	
def datastore_create(...

after
	 if records:
        data_dict['records'] = records
		if validcpr(data_dict) is not False:
            cprError()
			
def datastore_upsert(...

after
	 if records:
        data_dict['records'] = records
		if validcpr(data_dict) is not False:
            cprError()			