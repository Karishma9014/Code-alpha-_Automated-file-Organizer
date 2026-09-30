# Code-alpha-_Automated-file-Organizer
import os
import shutil
import re
import requests
from pathlib import Path

# ========== TASK 3 - OPTION 1: Move all .jpg files ==========
def move_jpg_files(source_folder="source", dest_folder="destination"):
    """Move all .jpg files from source to destination folder"""
    os.makedirs(source_folder, exist_ok=True)
    os.makedirs(dest_folder, exist_ok=True)
    
    count = 0
    for file in os.listdir(source_folder):
        if file.lower().endswith('.jpg') or file.lower().endswith('.jpeg'):
            src_path = os.path.join(source_folder, file)
            dest_path = os.path.join(dest_folder, file)
            shutil.move(src_path, dest_path)
            print(f"Moved: {file}")
            count += 1
    
    if count == 0:
        print(f"No .jpg files found in '{source_folder}' folder.")
        print(f"Tip: Put some .jpg files in '{source_folder}' and run again.")
    else:
        print(f"Total {count} files moved to '{dest_folder}'")

# ========== TASK 3 - OPTION 2: Extract emails from .txt file ==========
def extract_emails(input_file="input.txt", output_file="emails.txt"):
    """Extract all email addresses from a .txt file and save to another file"""
    
    # Create a sample input file if not exists (for demo)
    if not os.path.exists(input_file):
        with open(input_file, 'w') as f:
            f.write("""Contact us at support@codealpha.tech
            My personal mail is john.doe@gmail.com and office is john@company.com
            For queries: services.codealpha@gmail.com
            Invalid email @example.com
            Another one: test123@yahoo.co.in
            """)
        print(f"Created sample '{input_file}' for demo.")

    email_pattern = r'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}'
    
    try:
        with open(input_file, 'r', encoding='utf-8') as f:
            content = f.read()
        
        emails = re.findall(email_pattern, content)
        emails = list(set(emails))  # remove duplicates
        
        with open(output_file, 'w') as f:
            for email in emails:
                f.write(email + '\n')
        
        print(f"Found {len(emails)} email(s):")
        for e in emails:
            print(f" - {e}")
        print(f"Saved to '{output_file}'")
        
    except FileNotFoundError:
        print(f"Error: File '{input_file}' not found.")

# ========== TASK 3 - OPTION 3: Scrape title of webpage ==========
def scrape_webpage_title(url="https://www.python.org", output_file="title.txt"):
    """Scrape the title of a fixed webpage and save it"""
    try:
        print(f"Scraping: {url}")
        headers = {'User-Agent': 'Mozilla/5.0'}
        response = requests.get(url, headers=headers, timeout=10)
        response.raise_for_status()
        
        # Extract title using regex (no need for bs4)
        match = re.search(r'<title>(.*?)</title>', response.text, re.IGNORECASE | re.DOTALL)
        title = match.group(1).strip() if match else "No title found"
        
        print(f"Page Title: {title}")
        
        with open(output_file, 'w', encoding='utf-8') as f:
            f.write(f"URL: {url}\n")
            f.write(f"Title: {title}\n")
        
        print(f"Title saved to '{output_file}'")
        
    except Exception as e:
        print(f"Error scraping webpage: {e}")

# ========== MAIN MENU ==========
if __name__ == "__main__":
    while True:
        print("\n--- Task Automation with Python - TASK 3 ---")
        print("1. Move all .jpg files to new folder")
        print("2. Extract emails from .txt file")
        print("3. Scrape title of a webpage")
        print("4. Exit")
        
        choice = input("Enter your choice (1-4): ")
        
        if choice == '1':
            src = input("Enter source folder (default: source): ") or "source"
            dest = input("Enter destination folder (default: destination): ") or "destination"
            move_jpg_files(src, dest)
        elif choice == '2':
            inp = input("Enter input file (default: input.txt): ") or "input.txt"
            out = input("Enter output file (default: emails.txt): ") or "emails.txt"
            extract_emails(inp, out)
        elif choice == '3':
            url = input("Enter URL (default: https://www.python.org): ") or "https://www.python.org"
            scrape_webpage_title(url)
        elif choice == '4':
            print("Exiting...")
            break
        else:
            print("Invalid choice!")
