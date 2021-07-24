import argparse, os, json, tempfile
 
 
def wd(my_dict, key, val):
    if key in my_dict:
        try:
            my_dict[key].append(val)
        except AttributeError:
            my_dict[key] = [my_dict[key], val]
    else:
        my_dict[key] = val
 
 
parser = argparse.ArgumentParser()
parser.add_argument("-key", "--key", help=' random text')
parser.add_argument("-val", "--val", required=False, help='random_text')
args = parser.parse_args()
 
storage_path = os.path.join(tempfile.gettempdir(), 'storage.data')
 
if os.path.isfile('/tmp/storage.data'):
    if args.val:
        with open('/tmp/storage.data', 'r') as f:
            m = json.load(f)
            wd(m, args.key, args.val)
        with open('/tmp/storage.data', 'w') as f:
            json.dump(m, f)
 
    else:
        try:
            with open('/tmp/storage.data', 'r') as f:
                m = json.load(f)
                if m[args.key] is not list:
                    print(', '.join(m[args.key]))
                else:
                    print(m[args.key])
        except:
            print(None)
else:
    dicti = {}
    with open('/tmp/storage.data', 'w') as f:
        json.dump(dicti, f)
    if args.val is None:
        print(None)
    else:
        with open('/tmp/storage.data', 'r') as f:
            m = json.load(f)
            wd(m, args.key, args.val)
        with open('/tmp/storage.data', 'w') as f:
            json.dump(m, f)
