import sys
import os
import shutil
from pathlib import Path

CATEGORIES = extensions = {
    'images': ('.jpeg', '.png', '.jpg'),
    'videos': ('.avi', '.mp4', '.mov', '.mkv'),
    'documents': ('.doc', '.docx', '.txt', '.pdf', '.xlsx', '.pptx'),
    'audio': ('.mp3', '.wav', '.amr'),
    'archives': ('.rar', '.zip', '.gz', '.tar')
}


CYRILLIC_SYMBOLS = "абвгдеёжзийклмнопрстуфхцчшщъыьэюяєіїґ"
LATIN_SYMBOLS = ("a", "b", "v", "g", "d", "e", "e", "j", "z", "i", "j", "k", "l", "m", "n", "o", "p", "r", "s", "t", "u",
                 "f", "h", "ts", "ch", "sh", "sch", "", "y", "", "e", "yu", "ya", "je", "i", "ji", "g")

TRANS = {}
for cyrillic, latin in zip(CYRILLIC_SYMBOLS, LATIN_SYMBOLS):
    TRANS[ord(cyrillic)] = latin
    TRANS[ord(cyrillic.upper())] = latin.upper()

def translate(name):
    return name.translate(TRANS)


def delete_empty_folder(path: Path) -> None:
    for item in path.iterdir():
        if item.is_dir():
            delete_empty_folder(item)
            if not os.listdir(item):
                item.rmdir()


def unpack_archive(path: Path) -> None:
    for item in path.glob("archives/*"):
        if item.is_file():
            shutil.unpack_archive(str(item), extract_dir=str(item.parent))


def move_file(path: Path, root_dir: Path, categorie: str) -> None:
    target_dir = root_dir.joinpath(categorie)
    if not target_dir.exists():
        target_dir.mkdir()
    path.replace(target_dir.joinpath(f'{translate(path.stem)}{path.suffix}'))


def get_categories(path: Path) -> str:
    ext = path.suffix.lower()
    for cat, exts in CATEGORIES.items():
        if ext in exts:
            return cat
    return "Others"


def sort_folder(path: Path) -> None:
    for item in path.glob("**/*"):
        if item.is_file():
           cat = get_categories(item)
           move_file(item, path, cat)


def main():
    try:
        path = Path(sys.argv[1])
    except IndexError:
        return "No path to folder"
    
    if not path.exists():
        return f"Folder with path {path} does'nt exist"
    
    unpack_archive(path)
    sort_folder(path)
    delete_empty_folder(path)       
    
if __name__ == "__main__":
    print(main())